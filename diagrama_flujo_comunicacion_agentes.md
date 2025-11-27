flowchart TD
    Start([Inicio]) --> JudgeSetup[Juez: Configura API Server]
    JudgeSetup --> JudgeNgrok[Juez: Crea túnel ngrok]
    JudgeNgrok --> JudgeURL[Juez: Obtiene URL pública]
    JudgeURL --> JudgeServer[Juez: Inicia servidor FastAPI<br/>puerto 8000]
    JudgeServer --> JudgeWait[Juez: Espera mensajes]
    
    Start2([Inicio]) --> EntSetup[Emprendedor: Configura conexión]
    EntSetup --> EntSetURL[Emprendedor: Establece URL del Juez]
    EntSetURL --> EntCreatePitch[Emprendedor: Crea pitch inicial<br/>usando CrewAI]
    
    EntCreatePitch --> EntSend[Emprendedor: Envía mensaje POST<br/>/submit_message]
    EntSend --> JudgeReceive[Juez: Recibe mensaje]
    
    JudgeReceive --> JudgeCheck{¿Mensaje de<br/>Emprendedor?}
    JudgeCheck -->|Sí| JudgeAddHistory[Juez: Agrega a historial]
    JudgeAddHistory --> JudgeProcess1[Juez: Procesa con Judge Agent 1<br/>usando CrewAI]
    JudgeProcess1 --> JudgeAdd1[Juez: Agrega respuesta Judge 1<br/>al historial]
    JudgeAdd1 --> JudgeProcess2[Juez: Procesa con Judge Agent 2<br/>usando CrewAI]
    JudgeProcess2 --> JudgeAdd2[Juez: Agrega respuesta Judge 2<br/>al historial]
    JudgeAdd2 --> JudgeResponse[Juez: Retorna ambas respuestas<br/>JSON con Judge 1 y Judge 2]
    
    JudgeCheck -->|No| JudgeAck[Juez: Retorna ACK]
    
    JudgeResponse --> EntReceive[Emprendedor: Recibe respuestas]
    EntReceive --> EntAddHistory[Emprendedor: Agrega respuestas<br/>al historial local]
    EntAddHistory --> EntDisplay[Emprendedor: Muestra respuestas]
    
    EntDisplay --> EntDecision{¿Continuar<br/>conversación?}
    EntDecision -->|Sí| EntCreateResponse[Emprendedor: Crea respuesta<br/>usando CrewAI]
    EntCreateResponse --> EntSend
    EntDecision -->|No| End([Fin])
    
    JudgeAck --> JudgeWait
    JudgeWait --> JudgeReceive
    
    style JudgeSetup fill:#e1f5ff
    style JudgeServer fill:#e1f5ff
    style JudgeProcess1 fill:#fff4e1
    style JudgeProcess2 fill:#fff4e1
    style EntSetup fill:#e8f5e9
    style EntCreatePitch fill:#e8f5e9
    style EntCreateResponse fill:#e8f5e9
    style JudgeResponse fill:#fce4ec
    style EntReceive fill:#fce4ec
