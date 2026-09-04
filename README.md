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

## Lokalnie

Do lokalnego uruchomienia potrzebny jest Docker Desktop:

```powershell
docker compose up -d
```

Panel będzie dostępny pod `http://localhost:5678`. Zatrzymanie usługi:

```powershell
docker compose down
```
