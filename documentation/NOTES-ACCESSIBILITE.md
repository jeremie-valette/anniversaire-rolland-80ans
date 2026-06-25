# Notes — Accessibilité, RGAA, RGPD

Journal de décisions techniques pour la page Master du projet
`anniversaire-rolland-80ans`. À relire avant de finaliser le taux de
conformité affiché dans le bloc `.accessibilite` du HTML.

---

## Attributs ARIA — ce qui est utilisé, ce qui ne l'est pas, et pourquoi

### `aria-label` sur les liens de téléchargement → **utilisé**
Sans lui, un lecteur d'écran lit chaque `<div>` séparément
("01", "La Frise", "84 années une ligne de vie", "Télécharger"),
ce qui est fragmenté et peu clair à l'oreille.
Avec `aria-label="Télécharger La Frise, fichier JPG"` sur le `<a>`,
le lecteur d'écran annonce une phrase complète et cohérente.
Impact RGAA : un lien doit être compréhensible hors contexte visuel.

### `aria-expanded` → **non utilisé**
Sert uniquement sur un élément qui se déplie/replie (accordéon, menu).
On avait envisagé un bouton accordéon pour le bloc `.accessibilite`,
mais l'idée a été abandonnée (pas de JS prévu sur ce projet).
Sans comportement d'ouverture/fermeture, cet attribut n'a aucun sens
et induirait le lecteur d'écran en erreur.

### `aria-controls` → **non utilisé**, même raison
Cet attribut attend obligatoirement l'**ID** de l'élément qu'il
contrôle (pas un booléen `true`/`false`). Comme il n'y a pas
d'élément à contrôler (pas d'accordéon), il est inutile ici.

### `lang="fr"` sur `<html>` → **utilisé**
Indique au lecteur d'écran la langue de la page pour une
prononciation correcte. Obligatoire RGAA critère 8.3.

### `<header>`, `<main>`, `<footer>` → **suffisants sans `role=""` ARIA**
Le RGAA recommande le HTML natif plutôt que l'ARIA quand le HTML
natif fait déjà le travail. Ajouter `role="banner"` sur `<header>`
ou `role="main"` sur `<main>` serait redondant.

### `hidden` (attribut natif) → **non utilisé**
Aurait servi uniquement avec l'accordéon abandonné, pour cacher le
contenu repliable du DOM (pas juste visuellement — ce qui compte
aussi pour le lecteur d'écran). Sans accordéon, pas nécessaire.

---

## RGPD

Site statique, sans formulaire, sans champ `<input>`, sans cookie,
sans script tiers (analytics, publicité) → **aucune donnée
personnelle collectée**.

Point de vigilance retenu : pas de police chargée depuis un CDN
externe (ex. Google Fonts), qui enverrait l'IP du visiteur à un
tiers sans consentement. Polices système / auto-hébergées
uniquement.

---

## RGAA — à évaluer formellement

Le bloc `.accessibilite` du HTML affiche actuellement "À évaluer"
pour le taux RGAA — volontaire, en attendant un audit réel (calcul
de contraste couleur précis, vérification complète des critères)
plutôt qu'un chiffre arbitraire. Objectif assumé : un taux honnête,
même bas, plutôt qu'un affichage cosmétique.

Pistes déjà actées favorisant la conformité :
- Zones tactiles ≥ 64px (dépasse le minimum WCAG 2.5.5)
- `:focus-visible` géré pour la navigation clavier
- `prefers-reduced-motion` respecté
- Structure sémantique HTML5 (`header`/`main`/`footer`)

---

## Décisions abandonnées (et pourquoi)

### Bouton "Tout télécharger" (.zip)
Évoqué, puis écarté : nécessiterait un fichier zip à fabriquer et
maintenir, et le dézippage reste une étape supplémentaire pas
évidente pour une partie du public (invités âgés, peu à l'aise avec
leur téléphone). Les 5 boutons individuels restent la solution
principale, plus directe.

### QR codes multiples
Écarté dès la conception : un seul QR code, pointant vers la page
Master, imprimé sur les 5 objets physiques — évite d'avoir à générer
et gérer 5 QR codes différents.
