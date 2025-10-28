# Configuration Google Tag Manager

Ce guide vous explique comment configurer Google Tag Manager (GTM) avec Facebook Pixel, Google Analytics 4 (GA4) et Microsoft Clarity.

## 1. Configuration initiale de GTM

### 1.1 Créer un compte GTM
1. Allez sur [Google Tag Manager](https://tagmanager.google.com/)
2. Créez un compte et un conteneur pour votre site web
3. Récupérez votre ID GTM (format: `GTM-XXXXXXX`)
4. Ajoutez-le dans votre fichier `.env.local` :
   ```
   NEXT_PUBLIC_GTM_ID=GTM-XXXXXXX
   ```

## 2. Configuration des balises dans GTM

### 2.1 Google Analytics 4 (GA4)

#### Prérequis
- Créez une propriété GA4 sur [Google Analytics](https://analytics.google.com/)
- Récupérez votre Measurement ID (format: `G-XXXXXXXXXX`)

#### Configuration dans GTM
1. Dans GTM, allez dans **Balises** → **Nouvelle**
2. Nommez la balise: "GA4 - Configuration"
3. Configuration de la balise:
   - Type: **Google Analytics: GA4 Configuration**
   - ID de mesure: Votre `G-XXXXXXXXXX`
4. Déclencheur: **All Pages**
5. Enregistrez

#### Balise d'événement (optionnel)
Pour suivre des événements spécifiques:
1. Créez une nouvelle balise de type **Google Analytics: GA4 Event**
2. ID de configuration: Sélectionnez votre balise GA4 Configuration
3. Nom de l'événement: Ex: `page_view`, `button_click`, etc.
4. Configurez le déclencheur approprié

### 2.2 Facebook Pixel

#### Prérequis
- Créez un Pixel Facebook sur [Meta Events Manager](https://business.facebook.com/events_manager)
- Récupérez votre Pixel ID (format numérique, ex: `1234567890123456`)

#### Configuration dans GTM
1. Dans GTM, allez dans **Balises** → **Nouvelle**
2. Nommez la balise: "Facebook Pixel - Base"
3. Configuration de la balise:
   - Type: **HTML personnalisé**
   - HTML:
   ```html
   <script>
   !function(f,b,e,v,n,t,s)
   {if(f.fbq)return;n=f.fbq=function(){n.callMethod?
   n.callMethod.apply(n,arguments):n.queue.push(arguments)};
   if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
   n.queue=[];t=b.createElement(e);t.async=!0;
   t.src=v;s=b.getElementsByTagName(e)[0];
   s.parentNode.insertBefore(t,s)}(window, document,'script',
   'https://connect.facebook.net/en_US/fbevents.js');
   fbq('init', 'VOTRE_PIXEL_ID');
   fbq('track', 'PageView');
   </script>
   <noscript>
   <img height="1" width="1" style="display:none"
   src="https://www.facebook.com/tr?id=VOTRE_PIXEL_ID&ev=PageView&noscript=1"/>
   </noscript>
   ```
   - Remplacez `VOTRE_PIXEL_ID` par votre ID Pixel
4. Déclencheur: **All Pages**
5. Enregistrez

#### Événements Facebook (optionnel)
Pour suivre des conversions:
1. Créez une nouvelle balise **HTML personnalisé**
2. HTML:
   ```html
   <script>
   fbq('track', 'Lead'); // ou 'Purchase', 'AddToCart', etc.
   </script>
   ```
3. Configurez le déclencheur approprié (ex: sur la page de confirmation)

### 2.3 Microsoft Clarity

#### Prérequis
- Créez un projet sur [Microsoft Clarity](https://clarity.microsoft.com/)
- Récupérez votre Project ID

#### Configuration dans GTM
1. Dans GTM, allez dans **Balises** → **Nouvelle**
2. Nommez la balise: "Microsoft Clarity"
3. Configuration de la balise:
   - Type: **HTML personnalisé**
   - HTML:
   ```html
   <script type="text/javascript">
   (function(c,l,a,r,i,t,y){
       c[a]=c[a]||function(){(c[a].q=c[a].q||[]).push(arguments)};
       t=l.createElement(r);t.async=1;t.src="https://www.clarity.ms/tag/"+i;
       y=l.getElementsByTagName(r)[0];y.parentNode.insertBefore(t,y);
   })(window, document, "clarity", "script", "VOTRE_PROJECT_ID");
   </script>
   ```
   - Remplacez `VOTRE_PROJECT_ID` par votre ID de projet Clarity
4. Déclencheur: **All Pages**
5. Enregistrez

## 3. Variables personnalisées (optionnel)

Pour faciliter la gestion, vous pouvez créer des variables:

1. Allez dans **Variables** → **Nouvelle**
2. Créez des variables de type **Constante** pour:
   - GA4 Measurement ID
   - Facebook Pixel ID
   - Clarity Project ID
3. Utilisez ces variables dans vos balises avec la syntaxe: `{{nom_de_la_variable}}`

## 4. Test et débogage

1. Dans GTM, cliquez sur **Prévisualiser**
2. Entrez l'URL de votre site local ou de production
3. Vérifiez que toutes les balises se déclenchent correctement
4. Utilisez les extensions navigateur pour vérifier:
   - **Google Tag Assistant** pour GA4
   - **Facebook Pixel Helper** pour Facebook Pixel
   - Console du navigateur pour Clarity

## 5. Publication

Une fois les tests validés:
1. Dans GTM, cliquez sur **Envoyer**
2. Ajoutez un nom de version et une description
3. Cliquez sur **Publier**

## 6. Vérification

Après publication, vérifiez que les données arrivent:
- **GA4**: [Google Analytics](https://analytics.google.com/) → Rapports → Temps réel
- **Facebook**: [Events Manager](https://business.facebook.com/events_manager) → Test Events
- **Clarity**: [Dashboard Clarity](https://clarity.microsoft.com/) → Recordings/Heatmaps

## Notes importantes

- Les balises peuvent mettre quelques heures avant de commencer à collecter des données
- Assurez-vous de respecter le RGPD en ajoutant un banner de consentement
- Pour le RGPD, envisagez d'utiliser les fonctionnalités de consentement de GTM
- Testez toujours en mode prévisualisation avant de publier

## Ressources

- [Documentation GTM](https://support.google.com/tagmanager)
- [Documentation GA4](https://support.google.com/analytics)
- [Documentation Facebook Pixel](https://www.facebook.com/business/help/952192354843755)
- [Documentation Clarity](https://docs.microsoft.com/en-us/clarity/)
