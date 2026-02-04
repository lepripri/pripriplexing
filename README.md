# 🚀 Pripriplexing : Niveau 3 & 4 (Arduino Mega)

Le **Pripriplexing** est une méthode d'optimisation du multiplexage permettant de piloter des paires de LEDs (ou d'afficheurs) de polarités opposées (Anode Commune et Cathode Commune) sur un bus de données partagé.

## 📐 La Formule de "Pripri"

Après tests sur 12 LEDs avec une Arduino Mega, la formule de calcul des broches nécessaires a été validée :

$Pins = \frac{L}{2} + S$

*Où  est le nombre total de LEDs (ou d'afficheurs).*

## 🛠 Matériel & Configuration

* **Contrôleur :** Arduino Mega 2560.
* **Câblage actuel :** 9 pins pour 12 LEDs (6 paires CC/AC).
* **Résistances :** * **220Ω** (Configuration 5V actuelle - Vert).
* **47Ω** (Prévu pour futur passage en 3.3V / ESP32).
* **10kΩ** (Pull-down pour éliminer les couleurs fantômes/ghosting).


* **Affichage :** 20x Afficheurs 7 segments 0.56" Verts (10x 3191AS Anode / 10x 3191BS Cathode).

## 💻 Code de Base (POV Optimisé)

Le code fonctionne sans `delay()` pour exploiter la persistance rétinienne et supprimer tout scintillement.

```cpp
// --- CONFIGURATION ---
const int pinsRGB[] = {2, 3, 4}; // Bus de données
const int pinsCommuns[] = {5, 6, 7, 8, 9, 10}; // Pins de sélection

void setup() {
  for(int i=0; i<3; i++) pinMode(pinsRGB[i], INPUT);
  for(int i=0; i<6; i++) pinMode(pinsCommuns[i], INPUT);
}

void loop() {
  for (int i = 0; i < 6; i++) {
    allumerPaire(i); // Rafraîchissement ultra-rapide sans delay
  }
}

```

## 📈 Évolution : Niveau 4 (En attente de livraison AliExpress 📦)

Le passage au Niveau 4 implique la gestion de **20 afficheurs 7 segments**.

* [ ] Réception des 100 résistances (220R, 100R, 47R, 10k).
* [ ] Test unitaire des afficheurs verts (3191AS/BS).
* [ ] Création du bus de segments (8 lignes A-DP).
* [ ] Implémentation de la table de vérité 0-9.

## ⚠️ Notes de maintenance

* **Problème USB :** Ports USB du PC (ou de la carte) ayant des faux contacts. **Interdiction de manipuler la carte pendant le téléversement.**
* **Applications Mobiles :** Suite à un bug sur **Redmi A5**, les applications de notes (**Easy Markdown**, **Notally**, **Fast n Small Notes**) sont inaccessibles [cite: 2026-02-01]. **Toute la documentation doit être faite directement ici sur GitHub.**
