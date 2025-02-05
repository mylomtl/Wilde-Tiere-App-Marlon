

# KI-gestützte Tier-Bildgenerierung und Attributanalyse

## Projektübersicht
Dieses Projekt ist eine webbasierte Anwendung zur KI-gestützten Generierung und Analyse von Tierbildern. Die Anwendung ermöglicht es Benutzern, Bilder von verschiedenen Tieren zu generieren und diese anhand verschiedener Attribute wie Niedlichkeit, Aggressivität und Größe zu analysieren.

## Technologiestack
- **Frontend-Framework**: Svelte
- **Routing**: Hash-basiertes Routing
- **Styling**: CSS mit responsivem Design
- **KI-Integration**: Stable Diffusion API für Bildgenerierung

## Hauptfunktionen

### 1. Hauptseite ("Wilde Tiere")
- Einführungsseite mit thematischem Hintergrundbild
- Navigation zu Generator und Bildergalerie
- Responsive Design für verschiedene Bildschirmgrößen

### 2. Bildgenerator
- Interaktive Slider zur Steuerung verschiedener Tierattribute:
  - Niedlichkeit (Cuteness)
  - Aggressivität (Aggression)
  - Traurigkeit (Sadness)
  - Flauschigkeit (Fluffness)
  - Schuppen (Scales)
  - Federn (Feathers)
  - Größe (Size)
- Echtzeit-Bildgenerierung basierend auf Benutzereinstellungen
- Responsives Layout mit automatischer Anpassung für mobile Geräte

### 3. Bildergalerie (Recents)
- Anzeige aller generierten Bilder
- Sortierungsfunktionen:
  - Sortierung nach verschiedenen Attributen
  - Umschaltbare Sortierrichtung (Ascending/Descending)
- Detaillierte Attributanzeige für jedes generierte Bild
- Responsive Grid-Layout für optimale Darstellung

## Technische Details

### Komponenten-Struktur
1. **App.svelte**
   - Hauptkomponente
   - Routing-Logik
   - Globales Layout-Management

2. **Generator.svelte**
   - Bildgenerierungs-Interface
   - Slider-Steuerungen
   - API-Integration für Bildgenerierung

3. **Recents.svelte**
   - Galerie-Ansicht
   - Sortier- und Filterfunktionen
   - Responsive Grid-Layout