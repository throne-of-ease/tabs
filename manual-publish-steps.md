# Ruční publikace na GitHub Pages

Postup pro příští nasazení této aplikace:

1. Zkontroluj stav repozitáře:
   ```powershell
   git status -sb
   ```

2. Ověř, že jsou připravené jen zamýšlené změny. V tomto projektu se obvykle publikuje `index.html` a případně související dokumentace.

3. Přidej jen potřebné soubory:
   ```powershell
   git add index.html manual-publish-steps.md
   ```

4. Vytvoř krátký commit:
   ```powershell
   git commit -m "fix analog board"
   ```

5. Odesílej aktuální branch na `origin`:
   ```powershell
   git push -u origin main
   ```

6. Po úspěšném pushi počkej na workflow GitHub Pages. Nasazení běží automaticky z `.github/workflows/pages.yml`.

7. Když se push nepovede kvůli přihlášení, spusť:
   ```powershell
   gh auth login -h github.com
   gh auth status
   ```

8. Před dalším nasazením znovu ověř, že v pracovním stromu nezůstaly cizí nebo nechtěné změny.
