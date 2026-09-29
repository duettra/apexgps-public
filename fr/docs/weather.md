# Météo

ApexGPS peut afficher les conditions actuelles, une prévision horaire et un aperçu sur 7 jours pour n'importe quel point qui vous intéresse.

## Ce que vous verrez

La météo est **activée par défaut** depuis 1.32.2. Dès que vous avez un point GPS :

- Une petite pastille apparaît au-dessus de la barre de stats et affiche les conditions à votre position actuelle.
- Toucher un waypoint affiche une ligne « Météo ici » sur le panneau du waypoint.
- Toucher l'une ou l'autre surface ouvre une feuille avec le détail complet.

Quand la météo est active, votre latitude/longitude est envoyée à Open-Meteo (une API publique gratuite) à chaque requête de prévision. Si vous préférez qu'aucun appel réseau ne soit fait, ouvrez **Paramètres → Météo** et désactivez **Afficher la météo** — la pastille et la ligne « Météo ici » disparaissent, et ApexGPS cesse de contacter Open-Meteo.

## Ce que montre la pastille

La pastille alterne entre quelques états :

| État | Apparence | Signification |
|---|---|---|
| Récente | une icône météo, puis `24° · 12 km/h NE` | Mise à jour dans les 15 dernières minutes. |
| Vieillissante | `… · il y a 32 min` | Plus de 15 minutes mais probablement encore exacte. |
| Périmée / hors-ligne | `⚠ … · il y a 2 h` (grisée) | Plus d'une heure ou pas de connexion. Touchez la pastille pour rafraîchir manuellement. |

La pastille ne disparaît pas une fois la météo activée — elle change seulement d'apparence pour indiquer la fiabilité des données.

## La feuille de prévisions

Touchez la pastille (ou la ligne « Météo ici » d'un waypoint) pour ouvrir la feuille complète. De haut en bas :

- **Maintenant** — grande icône météo + température, « ressenti », et une ligne pour vent / humidité + précip max / point de rosée + UV / pression + coucher.
- **Bande de prévisions avec sélecteur 24h / 8h / 2h** — huit icônes (version jour ou nuit selon le lever / coucher local pour ce pas) avec la température, et en dessous la **probabilité de précipitations**. Le pourcentage n'apparaît qu'à partir de 20 %, pour qu'une prévision sèche reste lisible. Le petit sélecteur au-dessus change la fenêtre :
  - **24h** — toute la journée d'un coup d'œil, une cellule toutes les 3 heures.
  - **8h** — les huit prochaines heures, une cellule par heure.
  - **2h** — les deux prochaines heures par pas de 15 minutes, pour repérer une averse ou un orage sur le point d'arriver (jusqu'à ~45 minutes avant la vue horaire). L'option **2h** apparaît là où les données à 15 minutes existent (une grande partie de l'Europe et de l'Amérique du Nord) ; ailleurs vous ne verrez que **24h** et **8h**.
- **Tendance de pression** (vert) — courbe sur 24 heures, utile pour repérer un front qui approche.
- **Tendance d'humidité** (bleu clair) — courbe sur 24 heures.
- **Prochains 7 jours** — une rangée d'icônes par jour avec maxi/mini de la journée, et la probabilité de précipitations lorsqu'elle atteint 20 % ou plus.

Un bouton de rafraîchissement (↻) dans l'en-tête de la feuille contourne le cache et force une requête fraîche. Hors-ligne, les données précédentes restent affichées et l'indicateur « périmée » de la pastille reste visible.

## Prévisions sensibles à l'altitude sur les sommets

Si un waypoint a une altitude stockée (saisie manuellement, fixée par GPS ou importée depuis un GPX avec `<ele>`), cette altitude est envoyée à Open-Meteo afin que le modèle sache qu'il s'agit d'un sommet et non d'un point de vallée à la même lat / lon. Idem pour la pastille : elle utilise votre altitude GPS. Sur un sommet à 1500 m cela corrige typiquement la température de prévision de ~9 °C par rapport à une recherche purement par coordonnées.

## Auto-rafraîchissement

Quand la météo est activée, la pastille se met à jour toutes les 15 minutes tant que l'app est ouverte et en ligne. En mode avion, la pastille conserve les dernières données connues avec un indicateur « périmée » jusqu'à reconnexion.

## Lire les icônes

Les symboles météo sont dessinés par ApexGPS lui-même : ils sont donc **identiques sur tous les téléphones**. La
couleur porte le sens :

| Couleur | Signification |
|---|---|
| Bleu | Précipitations — bruine, pluie, neige fondue ou neige |
| Ambre | Ciel clair (le soleil ; la nuit, un ciel clair montre une lune grise simple) |
| Gris | Nuages ou brouillard |
| Rouge | Orage |

Le détail des icônes suit la prévision : pluie faible, pluie et pluie forte sont trois symboles distincts, de même
que neige, neige forte et neige fondue. Comme la probabilité de précipitations est aussi affichée en chiffres, vous
n'avez jamais à vous fier à un petit symbole seul.

Jusqu'à la version 1.47.1, ces symboles étaient des emojis fournis par le téléphone : deux appareils pouvaient donc
afficher des images différentes — voire aucune — pour la même prévision. Corrigé à partir de 1.48.0.

## Sources de données

Les prévisions viennent de **[Open-Meteo](https://open-meteo.com)**, une API publique gratuite qui combine ECMWF, GFS, ICON et d'autres modèles globaux de premier rang. Gratuite pour usage personnel, sans compte, sans clé d'API.

## Limitations

- **Pluie convective en zones arides** est difficile à prévoir pour tout modèle. Attendez-vous à des manques occasionnels sur les orages éclairs en zones comme les montagnes Hajar des Émirats. La probabilité reste honnête à ce sujet, mais les événements localisés peuvent tomber hors-grille.
- **Alertes de phénomènes violents** ne font pas partie de cette fonction. ApexGPS ne diffuse pas de notifications push à l'approche d'orages.
- **Météo de route** non prise en charge — les prévisions sont des points, pas « quel temps fera-t-il sur ce sentier dans 2 heures ». Vous pouvez toucher des waypoints individuels le long d'une route pour vérifier.
