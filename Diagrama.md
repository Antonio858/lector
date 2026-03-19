```mermaid
graph TD
    %% Estilos
    classDef inicio fin fill:#f9f,stroke:#333,stroke-width:2px;
    classDef proceso fill:#bbf,stroke:#333,stroke-width:1px;
    classDef subrutina fill:#dfd,stroke:#333,stroke-width:1px;
    classDef decision fill:#ff9,stroke:#333,stroke-width:1px;

    %% INICIO Y CONFIGURACIÓN
    Start([Inicio del Programa]) ::: inicio --> Config[Configurar Puertos: <br> ADCON1, TRISA, TRISB, TRISE <br> como salidas digitales] ::: proceso
    Config --> CicloPrincipal((CICLO <br> PRINCIPAL)) ::: inicio

    %% ==========================================
    %% FASE 1: ASCENDENTE
    %% ==========================================
    CicloPrincipal --> InitAsc[MOVLW 1 <br> MI_GRUPO = 1] ::: proceso
    
    InitAsc --> OtroGrupoSube>OTRO_GRUPO_SUBE: <br> MI_PASO = 1] ::: proceso
    
    OtroGrupoSube --> OtroPasoSube[[Llamar HACER_MASCARA <br> Llamar PRENDER_LEDS <br> Llamar ESPERAR_UN_RATO]] ::: subrutina
    
    OtroPasoSube --> CondPasoSube{¿MI_PASO == MI_GRUPO?} ::: decision
    
    CondPasoSube -- NO --> IncPaso[MI_PASO = MI_PASO + 1] ::: proceso
    IncPaso --> OtroPasoSube
    
    CondPasoSube -- SI --> YaLlegoSube[[Llamar APAGAR_LEDS <br> Llamar ESPERAR_UN_RATO]] ::: subrutina
    
    YaLlegoSube --> IncGrupoSube[MI_GRUPO = MI_GRUPO + 1] ::: proceso
    
    IncGrupoSube --> CondGrupoSube{¿MI_GRUPO == 17?} ::: decision
    
    CondGrupoSube -- NO (Aún faltan LEDs) --> OtroGrupoSube

    %% ==========================================
    %% FASE 2: DESCENDENTE
    %% ==========================================
    CondGrupoSube -- SI (Terminó de subir) --> InitDesc[MOVLW 15 <br> MI_GRUPO = 15] ::: proceso
    
    InitDesc --> OtroGrupoBaja>OTRO_GRUPO_BAJA: <br> PUNTO_INICIO = MI_GRUPO + 1 <br> MI_PASO = PUNTO_INICIO] ::: proceso
    
    OtroGrupoBaja --> OtroPasoBaja[[Llamar HACER_MASCARA <br> Llamar PRENDER_LEDS <br> Llamar ESPERAR_UN_RATO]] ::: subrutina
    
    OtroPasoBaja --> CondPasoBaja{¿MI_PASO == MI_GRUPO?} ::: decision
    
    CondPasoBaja -- NO --> DecPaso[MI_PASO = MI_PASO - 1] ::: proceso
    DecPaso --> OtroPasoBaja
    
    CondPasoBaja -- SI --> YaLlegoBaja[[Llamar APAGAR_LEDS <br> Llamar ESPERAR_UN_RATO]] ::: subrutina
    
    YaLlegoBaja --> CondGrupoBaja{¿MI_GRUPO == 0?} ::: decision
    
    CondGrupoBaja -- NO (Aún no llega a 0) --> DecGrupoBaja[MI_GRUPO = MI_GRUPO - 1] ::: proceso
    DecGrupoBaja --> OtroGrupoBaja
    
    CondGrupoBaja -- SI (Terminó de bajar) --> TerminoTodo([Regresar a CICLO PRINCIPAL]) ::: inicio
    TerminoTodo -.-> CicloPrincipal
```
