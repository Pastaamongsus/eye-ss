# eye-ss
tool ss
@echo off
cls

rem Impostazioni predefinite
set "testo=PC CHECK"
set "colore=1" & rem Colore predefinito: blu

:scelta_colore
cls
echo Scegli il colore per il testo:
echo 1. Blu
echo 2. Verde
echo 3. Rosso
echo 4. Giallo
echo 5. Bianco

set /p "colore_scelta=Inserisci il numero del colore desiderato: "

rem Assegna il codice di colore in base alla scelta dell'utente
if "%colore_scelta%"=="1" set "colore=1"
if "%colore_scelta%"=="2" set "colore=2"
if "%colore_scelta%"=="3" set "colore=4"
if "%colore_scelta%"=="4" set "colore=6"
if "%colore_scelta%"=="5" set "colore=f"

rem Modifica del colore del testo
color %colore%

rem Posizione predefinita del testo
set "posizione=5 2" 

:scelta_posizione
cls
echo Scegli la posizione per il testo:
echo 1. In alto a sinistra
echo 2. Al centro
echo 3. In basso a sinistra

set /p "posizione_scelta=Inserisci il numero della posizione desiderata: "

rem Assegna la posizione del testo in base alla scelta dell'utente
if "%posizione_scelta%"=="1" set "posizione=5 2"
if "%posizione_scelta%"=="2" set "posizione=13 7"
if "%posizione_scelta%"=="3" set "posizione=23 20"

rem Pulisce lo schermo e imposta la posizione del testo
cls
echo === %testo% ===
goto animazione_caricamento

:animazione_caricamento
echo.
echo Caricamento in corso...

:: Attendi 3 secondi prima di procedere
ping 127.0.0.1 -n 3 > nul

rem Animazione di caricamento
setlocal EnableDelayedExpansion
set "spinner=|/-\"
for /l %%i in (1,1,20) do (
    cls
    echo === %testo% ===
    echo.
    echo Caricamento in corso... !spinner:~%%i,1!
    ping 127.0.0.1 -n 1 > nul
    ping 127.0.0.1 -n 1 > nul
    ping 127.0.0.1 -n 1 > nul
)
cls

echo === %testo% ===
echo.

echo - Verifica dello spazio disco -
echo ------------------------------
wmic logicaldisk get caption,description,filesystem,size,freespace

echo.
echo - Informazioni sulla memoria -
echo ------------------------------
wmic memorychip get capacity
wmic memorychip get speed

echo.
echo - Informazioni sulla CPU -
echo ---------------------------
wmic cpu get name, caption, maxclockspeed

echo.
echo - Informazioni sulla scheda madre -
echo -----------------------------------
wmic baseboard get product, manufacturer, version

echo.
echo - Informazioni sul sistema operativo -
echo --------------------------------------
wmic os get caption, version, osarchitecture

echo.
echo - Elenco dei programmi installati -
echo ------------------------------------
wmic product get name, version

echo.
echo === Ricerca del file cheto.exe ===
echo.

dir /s /b "C:\cheto.exe" || echo Il file cheto.exe non è stato trovato.

echo.
echo === Controllo dei file ===
echo.

robocopy /l /e /njh /njs /ndl /fp /xx /log:"C:\cambiamenti.log" "C:\origine" "C:\destinazione" > nul

if errorlevel 16 (
    echo Sono stati trovati nuovi file.
) else if errorlevel 8 (
    echo Sono stati trovati file modificati.
) else if errorlevel 4 (
    echo Sono stati trovati file eliminati.
) else (
    echo Nessuna modifica rilevata.
)

echo.
echo.
echo === Visualizzazione dei file recenti ===
echo.

set /p "scelta=Vuoi visualizzare l'elenco dei file recenti? [si/no]: "

if /i "%scelta%"=="si" (
    echo Elenco dei file recenti:
    dir /b /a-d "%userprofile%\AppData\Roaming\Microsoft\Windows\Recent"
) else (
    echo Scelta non valida o non desideri visualizzare l'elenco dei file recenti.
)

echo.
echo === Copia dei file dalla cartella %temp% ===
echo.

echo Elenco dei file nella cartella %temp%:
for %%i in ("%temp%\*") do (
    echo %%i
)

echo.
echo === Copia dei file dalla cartella Prefetch ===
echo.

echo Elenco dei file nella cartella Prefetch:
for %%i in ("%SystemRoot%\Prefetch\*") do (
    echo %%i
)

echo.
echo === Fine del controllo ===
pause
