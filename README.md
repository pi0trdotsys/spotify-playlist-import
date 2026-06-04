# spotify-playlist-import

Przydatne przy zmianie konta Spotify — automatycznie przenosi polubione utwory.

## Jak to działa

1. Eksportujesz polubione utwory ze starego konta do CSV
2. Uruchamiasz skrypt na nowym koncie
3. Skrypt dodaje wszystkie utwory do polubionych

---

## Krok 1 — Eksport z exportify

Wejdź na https://exportify.net/, zaloguj się starym kontem i pobierz **Liked Songs** jako CSV.  
Zapisz plik jako `Liked_Songs.csv` w katalogu projektu.

---

## Krok 2 — Utwórz aplikację w Spotify Developer Dashboard

1. Wejdź na https://developer.spotify.com/dashboard/create
2. Wypełnij formularz (nazwa i opis dowolne)
3. W polu **Redirect URIs** wpisz dokładnie: `http://127.0.0.1:8888/callback`
4. W sekcji **APIs used** zaznacz tylko: **Web API** ✅  
   (Web Playback SDK, Android, iOS, Ads API — **nie są potrzebne**)
5. Zapisz i wejdź w ustawienia aplikacji — skopiuj **Client ID** i **Client Secret**

---

## Krok 3 — Konfiguracja `.env`

Skopiuj plik przykładowy i uzupełnij swoje dane:

```bash
cp .env.example .env
```

Edytuj `.env`:

```env
SPOTIFY_CLIENT_ID=tutaj_wklej_client_id
SPOTIFY_CLIENT_SECRET=tutaj_wklej_client_secret
SPOTIFY_REDIRECT_URI=http://127.0.0.1:8888/callback
CSV_FILE=Liked_Songs.csv
```

---

## Krok 4 — Instalacja zależności

```bash
pip install spotipy pandas python-dotenv
```

---

## Krok 5 — Uruchomienie

```bash
python3 spotify_import_with_credentials.py
```

Przy pierwszym uruchomieniu otworzy się przeglądarka z prośbą o autoryzację.  
Po zalogowaniu zostaniesz przekierowany na `http://127.0.0.1:8888/callback` — to normalne.

---

## Rozwiązywanie problemów

### `SpotifyOauthError: invalid_client`

Spotipy cache'uje token w pliku `.cache`. Jeśli token wygasł lub dane aplikacji się zmieniły, odświeżenie nie działa.

**Rozwiązanie:** usuń cache i uruchom ponownie:

```bash
rm -f .cache && python3 spotify_import_with_credentials.py
```

Zostaniesz poproszony o ponowne zalogowanie przez przeglądarkę.

### Przeglądarka się nie otwiera

Skopiuj URL wypisany w terminalu i wklej go ręcznie do przeglądarki.

---

## .gitignore

Upewnij się, że `.env` i `.cache` są w `.gitignore` — nie wrzucaj credentiali na GitHub:

```
.env
.cache
```
