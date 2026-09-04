# n8n w GitHub Codespaces

Repozytorium uruchamia prywatną instancję [n8n](https://n8n.io/) w GitHub Codespaces. Każdy Codespace ma własny wolumin Docker, więc workflowy i poświadczenia ucznia nie są współdzielone.

## Start dla ucznia

1. Na stronie repozytorium wybierz **Code** > **Codespaces** > **Create codespace on main**.
2. Poczekaj, aż Codespaces zakończy konfigurację kontenera. n8n uruchamia się automatycznie.
3. Otwórz kartę **PORTS** w terminalu VS Code i wybierz ikonę przeglądarki przy porcie `5678` (n8n). Port jest prywatny, dostępny wyłącznie dla właściciela Codespace.
4. Przy pierwszym wejściu utwórz konto właściciela n8n.

## Zatrzymywanie i ponowne uruchamianie

### Zatrzymanie po lekcji

1. Otwórz [github.com/codespaces](https://github.com/codespaces).
2. Odszukaj swój Codespace dla repozytorium `n8n-codespaces`.
3. Kliknij przy nim menu **...**, a następnie **Stop codespace**.

Nie wybieraj **Delete codespace**, ponieważ usuwa on również wolumin `n8n_data` z workflowami i poświadczeniami.

### Ponowne uruchomienie

- Ponowne uruchomienie tego samego Codespace zachowuje dane n8n w woluminie `n8n_data`.
- Nowy Codespace zaczyna od pustej instancji n8n.

## Uwagi dla prowadzącego

- Uczniowie powinni tworzyć własne Codespaces, a nie pracować w jednym wspólnym środowisku.
- Nie publikuj portu `5678` jako publicznego, szczególnie gdy workflowy zawierają poświadczenia.
- Usługi z webhookami zewnętrznymi wymagają stałego publicznego adresu. Tymczasowy adres Codespaces jest właściwy do ćwiczeń w panelu, ale nie do trwałych integracji produkcyjnych.

## Uruchomienie lokalne w Windows

Poniższe kroki uruchamiają n8n na własnym komputerze pod adresem `http://localhost:5678`. Wymagany jest Windows 10/11 64-bit, włączona wirtualizacja w BIOS/UEFI i co najmniej 8 GB pamięci RAM.

### 1. Zainstaluj lub zaktualizuj WSL 2

1. Otwórz **PowerShell jako administrator**.
2. Wpisz polecenie:

	```powershell
	wsl --install
	```

3. Uruchom komputer ponownie, jeśli Windows o to poprosi.
4. Po ponownym uruchomieniu sprawdź wersję:

	```powershell
	wsl --version
	```

Jeżeli WSL jest już zainstalowany, zamiast pierwszego polecenia użyj `wsl --update`.

### 2. Zainstaluj i uruchom Docker Desktop

1. Pobierz instalator z [oficjalnej strony Docker Desktop](https://www.docker.com/products/docker-desktop/).
2. Uruchom pobrany instalator i przy wyborze backendu pozostaw zaznaczoną opcję **Use WSL 2 instead of Hyper-V**.
3. Zakończ instalację, a następnie otwórz z menu Start aplikację **Docker Desktop**.
4. Przy pierwszym uruchomieniu zaakceptuj warunki Docker Desktop i poczekaj, aż aplikacja pokaże, że silnik Docker działa.
5. Upewnij się, że Docker używa **Linux containers**. Obrazy n8n są kontenerami Linux.

### 3. Pobierz repozytorium

W PowerShell uruchom:

```powershell
git clone https://github.com/ZSZiP-Kroscienko/n8n-codespaces.git
Set-Location n8n-codespaces
```

Alternatywnie na stronie repozytorium wybierz **Code** > **Download ZIP**, rozpakuj archiwum i otwórz PowerShell w rozpakowanym katalogu.

### 4. Przygotuj ustawienia dla localhost

W tym samym katalogu utwórz plik `.env` z jedną linią:

```text
N8N_SECURE_COOKIE=false
```

To ustawienie jest potrzebne lokalnie, ponieważ panel działa przez HTTP. Plik `.env` nie jest wysyłany do GitHub.

### 5. Sprawdź Docker i uruchom n8n

Najpierw sprawdź, czy Docker Desktop działa:

```powershell
docker version
docker compose version
```

Następnie uruchom n8n:

```powershell
docker compose up -d
docker compose ps
```

Pierwsze uruchomienie może potrwać kilka minut, ponieważ Docker pobiera obraz n8n. Status usługi powinien być `running`.

### 6. Otwórz panel n8n

Wejdź w przeglądarce na [http://localhost:5678](http://localhost:5678). Przy pierwszym wejściu utwórz konto właściciela. Workflowy i poświadczenia są zapisane w lokalnym woluminie Docker `n8n_data`.

### Zatrzymanie i ponowne uruchomienie

Po zakończeniu pracy zatrzymaj kontener, zachowując dane:

```powershell
docker compose stop
```

Przy kolejnym użyciu uruchom go ponownie:

```powershell
docker compose up -d
```

Polecenie `docker compose down` również zachowuje nazwany wolumin `n8n_data`, ale usuwa kontener i sieć. Nie używaj `docker compose down -v`, chyba że celowo chcesz usunąć wszystkie workflowy i poświadczenia.

### Gdy coś nie działa

- Gdy `docker version` zwraca błąd połączenia, otwórz Docker Desktop i poczekaj na uruchomienie silnika.
- Gdy port `5678` jest zajęty, zamknij program używający tego portu albo zmień lewe `5678` w `docker-compose.yml` na wolny port, na przykład `5679:5678`.
- Aby podejrzeć logi n8n, uruchom `docker compose logs -f n8n`. Naciśnij `Ctrl+C`, aby zakończyć podgląd logów; kontener nadal będzie działał.
