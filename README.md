# Joc al PIC18F45K22

> Creat durant el desembre de 2022

>  **Autors:**
> - Sergi Martínez [SMP03](https://github.com/SMP03/)
> - Guillermo Medina [Raiward9](https://github.com/Raiward9/)

## Funcionament del joc
Primer de tot es mostren els crèdits inicials i l’splash inicial. Després es pot seleccionar la dificultat (velocitat del joc) amb els botons E (augmentar dificultat), F (disminuir dificultat) i K (seleccionar dificultat). Un cop dins del joc a dalt a la dreta es mostra el tipus de controlador. Amb el botó K es pot anar canviant entre Keypad, Joystick i Keyboard. L’objectiu del joc és aconseguir les monedes (50pts) i les gemes (100pts) necessàries per aconseguir 500 punts abans que s’acabi el temps (60s) o les col·lisions amb els obstacles matin al jugador (3 vides).

## Estructura del codi
El codi segueix una estructura modular, on cada perifèric o grup de funcions específiques es troben en el seu propi arxiu.
### Llibreria GLCD bàsica:
- ```ascii.h```: arxiu amb els bitmaps per a cada caràcter ascii
- ```splash.h```: arxiu amb el bitmap per a l’splash inicial
- ```GLCD.h```: llibreria amb les comandes bàsiques per pintar i escriure a la GLCD.
### Llibreries GLCD avançades:
- ```animation.h```: conté les funcions necessàries per a les pantalles inicials i finals
- ```graphics.h```: conté tot el codi relacionat amb l’escriptura d’sprites a pantalla (té diversos modes d’escriptura i neteja implementats).
### Llibreries d’altres perifèrics:
- ```ioserial.h```: conté les funcions per a comunicar-se amb el teclat mitjançant el mòdul EUSART (mode de control del jugador per teclat)
- ```joystick.h```: conté les funcions per implementar el moviment del jugador mitjançant un joystick amb mapeig lineal de posició del joystick a velocitat del jugador.
- ```keypad.h```: conté les funcions per implementar el moviment del jugador mitjançant quatre botons direccionals.
- ```buzzer.h```: conté les funcions per utilitzar un buzzer per fer els diferents sons i melodies del joc.
### Llibreries de lògica del joc:
- ```sprite.h```: Conté l’struct de la unitat bàsica del joc, l’sprite, que es fa servir tant per a representar el jugador com els obstacles i objectius.
- ```gamehandler.h```: Conté tota la lògica involucrada en el joc (llevat dels controls).
### Arxiu principal
- ```main```: inicialització de tots els mòduls i transferència d’informació entre els controladors i el joc.

## Problemes trobats i solucions:
**Problema**: l’ús de la GLCD no permet ISR que tardin massa.
> **Solució**: ús de flags per comunicar-se amb el bucle principal.

**Problema**: cal comunicar-se amb molts perifèrics diferents.
> **Solució**: gestionar la comunicació amb cada mòdul des de funcions aïllades i fer les crides des del main.c.
	
**Problema**: les operacions de la GLCD són molt lentes i limiten el tipus d’accés.
> **Solució**: optimitzar les crides a la GLCD i sempre treballar en termes de bytes i fer servir operacions de bits per obtenir el resultat desitjat.

**Problema**: la lògica de control del joc involucra controlar molts objectes alhora.
- **Solució**: Gestionar la informació de cada sprite en un struct i fer estructures de dades escalables sobre aquests structs.
