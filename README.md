# Documentation Technique : Le Pripriplexing

Le **Pripriplexing** est une architecture d'affichage hybride optimisée pour piloter des LEDs RGB (mélange de Cathode Commune et Anode Commune) avec un nombre minimal de broches de microcontrôleur.

## 1. Concept Fondamental

Le système repose sur la fusion de deux techniques :

* **Charlieplexing des Communs** : Les broches communes (Anodes ou Cathodes) sont montées en réseau Charlieplexé.
* **Multiplexage des Couleurs** : Les lignes de couleurs (R, V, B) sont partagées sur un bus simple.

Cette méthode permet de piloter **18 segments (6 LEDs RGB)** avec seulement **6 broches** GPIO.

---

## 2. Architecture Matérielle

### Configuration des Broches (Arduino Mega)

| Broches | Fonction | Description |
| --- | --- | --- |
| **D2, D3, D4** | Bus RGB | Couleurs plexées (Rouge, Vert, Bleu) |
| **D5, D6, D7** | Communs | Adressage Charlieplexé (P1, P2, P3) |

### Montage des LEDs par Paires

Le "Pripriplexing" utilise l'opposition de polarité pour doubler la capacité de chaque broche de commun :

* **Paire 1 (D5)** : Une LED Cathode Commune (CC) + Une LED Anode Commune (AC).
* **Paire 2 (D6)** : Une LED CC + Une LED AC.
* **Paire 3 (D7)** : Une LED CC + Une LED AC.

> **Note Cruciale :** Des résistances de **1kΩ** sont obligatoires sur chaque ligne de couleur (D2, D3, D4) pour limiter le courant et supprimer le ghosting par chute de tension.

---

## 3. Logique de Pilotage (Table de Vérité)

Pour allumer une LED spécifique, les broches non utilisées doivent impérativement rester en **Haute Impédance (INPUT)**.

| Cible | Type | Commun (D5-D7) | Couleur (D2-D4) |
| --- | --- | --- | --- |
| **LED CC** | Cathode Commune | `OUTPUT LOW` (Masse) | `OUTPUT HIGH` (5V) |
| **LED AC** | Anode Commune | `OUTPUT HIGH` (5V) | `OUTPUT LOW` (Masse) |

---

## 4. Synthèse des Couleurs (7 Couleurs)

Le Pripriplexing utilise la persistance rétinienne pour créer des couleurs secondaires :

1. **Rouge** : Allumage R.
2. **Vert** : Allumage V.
3. **Bleu** : Allumage B.
4. **Jaune** : Alternance rapide R + V.
5. **Cyan** : Alternance rapide V + B.
6. **Magenta** : Alternance rapide R + B.
7. **Blanc** : Alternance rapide R + V + B.

### 📝 Note

Contrairement à ce que l'on pourrait croire, une LED RGB dans cette configuration n'a pas systématiquement besoin d'alternance rapide :

* **Couleurs Primaires (Rouge, Vert ou Bleu) :** L'allumage est **statique**. Tant que la broche de commun et la broche de couleur choisie sont actives, la LED brille de façon continue sans aucun scintillement.
* **Couleurs Composées (Jaune, Cyan, Magenta, Blanc) :** C'est ici que l'alternance intervient. Puisque les trois anodes (ou cathodes) de la LED RGB partagent le même point de retour, l'Arduino doit basculer très vite entre les broches de couleur pour donner l'illusion d'un mélange.
---

## 5. Avantages Constatés

* **Zéro Ghosting** : Grâce à l'opposition CC/AC et aux résistances de 1kΩ.
* **Stabilité du Blanc** : Le mélange des trois couleurs reste net malgré le multiplexage.
* **Efficience** : Consommation électrique réduite par le balayage temporel.

---

*Document généré le 3 février 2026. Prototype conservé en lieu sûr (coffre-fort). ©2025 le pripri • tout droits réservé*
