# Serwer aplikacji Biomarkers Detector

Instrukcja pokazująca jak krok po kroku przygotować serwer do działania dla aplikacji mobilnej Biomarkers Detector

## 1. Środowisko programistyczne

Zainstaluj środowisko na którym będzie działał Twój serwer z frameworkiem Flask może być to Pycharm, VSC (Visual Studio Code) albo inne środowisko analizujące kod Pythona. Zainstaluj też odpowiednią wersje języka Python (3.10 lub 3.12)

## 2. Serwer z frameworkiem Flaks

2.1 Zainstaluj i stwórz nowy projekt Flask w swoim środowisku:
```sh
pip install Flask
```
W razie potrzeby skorzystaj z poradnika: [Wprowadzenie do Flask](https://brylka.net/hello-world-instalacja-i-pierwsze-uruchomienie-flask)

2.2 Pobierz z repozytorium pliki: app.py, model.py, mel-model.pth oraz nagranie_testowe.wav oraz dołącz je do wcześniej stworzonego projektu 

2.3 Zainstaluj niezbędne biblioteki to prawidłowego działania kodu m.in. librosa, matplotlib, PIL czy torch 
np.:
```sh
pip install librosa
```

2.4 (Opcjonalnie) Stwórz folder o nazwie "uploads" w głównym drzewie projektu (będzie on przechowywał nagrania audio oraz spektrogramy naszej aplikacji, jeśli go nie stworzysz powinien sam się utworzyć przy pierwszy skorzystaniu z aplikacji)

## 3. FFMPEG 

Jest to niezbędny biblioteka do działania biblioteki libros aby sepktrogramy poprawnie się generowały

3.1 Pobierz bibliotekę, zainstaluj oraz zapisz w wiadomym dla siebie miejscu: [pobierz ffmpeg](https://www.ffmpeg.org/download.html)

3.2 Dołącz bibliotekę do zmiennych środowiskowych:
Dla Windows:
	1. Wejdź do zmiennych środowiskowych PATH: System -> Informacje -> Zaawansowane ustawienia systemu -> Zmienne środowiskowe -> 	Path -> dodaj nową zmienną 

	2. Dodaj nową zmienną: {ścieżka do zapisanej biblioteki}\ffmpeg-master-latest-win64-gpl\ffmpeg-master-latest-win64-gpl\bin

3.3 Po uruchomieniu aplikacji powinna się sama skonfigurować z modelem 

## 4. Test działania serwera

Jeśli wszystko się powiodło serwer powinien działać poprawnie natomiast warto wykonać poniższy test:

4.1 Wejdź do model.py, odkomentuj kod na samym dole oraz sprawdź czy otrzymasz wynik (jeśli tak model działa poprawnie, w przeciwnym razie, należy naprawić błąd który się pojawił)

4.2 Wejdź do terminala w środowisku i wpisz komendę uruchomienia serwera:
```sh
python app.py
```
Jeśli otrzymasz:
```sh
 * Serving Flask app 'app'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on all addresses (0.0.0.0)
 * Running on http://127.0.0.1:5000
 * Running on http://{adres lokalny komputera}:5000
Press CTRL+C to quit
```
Serwer działa poprawnie oraz można kliknąć w link i powinien pojawić się komunikat 'Server working correct!'

Jeśli natomiast brakuje adresu lokalnego komputera możliwe że należy skonfigurować ustawienia firewalla:
	4.2.1 Wejdź do ustawień firewall: Panel sterowania -> System i zabezpieczenia -> Zapora Windows Defender -> ustawienia 	zaawansowane

	4.2.2 Ustaw nową regułę przychodzącą i wychodzącą dla portu 5000 i protokołu TCP

## 5. Serwer Lokalny oraz Serwer Globalny (zdalny)

Aplikacja działa dla 3 rodzajów serwera:
- Localny (localhost)
- Globalny (Adres tymczasowy)
- Globalny (Adres domeny)

### 5.1 Localny (localhost) 

Działa on jeśli serwer oraz telefon z aplikacją mobilną znajdują się w tej samej sieci WiFi. Znajdziesz go po uruchomieniu serwera pod {adres lokalny komputera} albo po wejściu do cmd (command window) i wpisaniu "ipconfig" znajduje się on pod IPv4 Address.

Należy do wpisać w aplikacji w oknie: Adres IP serwera (eng. Server IP Address)


### 5.2 Global (ngrok)

Do serwera globalnego niezbędna będzie instalacja aplikacji ngrok: [Pobierz ngrok](https://ngrok.com/download)
Tworzy on tunel dzięki której możemy korzystać z naszego serwera zdalnie. 
Jak z niego korzystać oraz pobrać można zobaczyć tutaj: [ngrok youtube](https://www.youtube.com/watch?v=palp_5k81j0&t=297s)

Pobrany plik należy wypakować oraz uruchomić konsolę (warto ją sobie umieścić w widocznym miejscu na pulpicie)
Przy pierwszym włączeniu należy podać swój token uwierzytelniający [token](https://dashboard.ngrok.com/tunnels/authtokens)
Następnie możemy korzystać z ngroka na dwa sposoby: 

5.2.1 Adres tymczasowy

Pozwala korzystać z ngroka i serwera natomiast trzeba podawać do aplikacji numer za każdym razem: 
Wpisz w konsoli:
```sh
ngrok http 5000
```
Przykładowy adres:
https://8400-178-111-122-139.ngrok-free.app

Należy wpisać do aplikacji:
Adres ngrok (Ngrok Address): 8400
Adres IP serwera ngrok: 178-111-122-139

ps. Aplikacja zapamiętuje dane więc nie trzeba wpisywać tego za każdym ponownym uruchomieniem aplikacji natomiast po restarcie serwera adres ngrok ulegnie zmianie

5.2.2 Adres domeny

Pozwala na korzystanie z ngroka za pomocą domeny
Po wejściu na [adresy domen](https://dashboard.ngrok.com/cloud-edge/domains) ngrok generuje nam losową domenę dla naszego konta 
która nie ulega zmianie. Teraz serwer ngrok powinien być uruchamiany następująco:
```sh
ngrok http --domain={domena}.ngrok-free.app 5000
```
Nazwę domeny należy wpisać w aplikacji:
Domena(domain): …

## 6. Opcje dodatkowe

- Jeśli nie chcesz za każdym razem wpisywać komend tylko uruchamiać serwer jednym kliknięciem polecam stworzyć sobie skróty uruchamiające skrypt, należy w notatniku napisać odpowiednie komendy i zapisać plik .bat:
```sh
@echo off
cd C:\Users\filip\OneDrive\Pulpit\magisterka\PD_flask_flutter (ścieżka do serwera flask)
python app.py
pause
```
Tak samo można zrobić z ngrokiem

- Jeśli chciałbyś mieć dostęp do twojego komputera zdalnie i móc odpalić serwer z każdego miejsca polecam zastosować TeamViewera 

 





