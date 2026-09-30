# 📋 suivi-portfolio

> ### 🌐 [Nills Maillet — Portfolio](https://maillet-nills.github.io)
> 
> Développement web, réseaux & cybersécurité (BTS SIO SLAM) `maillet-nills.github.io`

Ce dépôt est un **vault Obsidian** qui sert à suivre l'avancement de mon portfolio et la traçabilité de mes compétences (BTS SIO, épreuve E4).

---

## 📁 Contenu

- `dashboard.md` : acquisition des savoirs du Bloc 1 (S1 à S18), avec preuves.
- `todolist.md` : tâches du portfolio, page par page.

---

## 💡 Pourquoi ouvrir ce dépôt avec Obsidian ?

Sur GitHub, les fichiers sont lisibles mais figés. Avec Obsidian, c'est plus agréable et plus rapide :

- les cases à cocher `- [ ]` se cochent d'un clic, directement dans la note ;
- les liens entre fichiers fonctionnent comme un vrai site (navigation d'une note à l'autre) ;
- on édite et on voit le rendu en même temps ;
- on peut ajouter des plugins (calendrier, Gantt, etc.) selon les besoins.

---

## 🚀 Ouvrir le vault

1. Installer [Obsidian](https://obsidian.md).
2. Récupérer le dépôt :
    
    ```bash
    git clone https://github.com/maillet-nills/suivi-portfolio.git
    ```
    
    (ou bouton vert **Code** → **Download ZIP** sur GitHub, puis décompresser)
3. Dans Obsidian : **Ouvrir un dossier en tant que coffre** (_Open folder as vault_).
4. Sélectionner le dossier `suivi-portfolio`.

Le dashboard et la todolist sont alors directement disponibles dans la barre latérale.

---

## 🔄 Sauvegarder les changements

Après avoir coché ou modifié des notes :

```bash
git add .
git commit -m "docs: update tracking"
git push
```