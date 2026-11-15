# Gymloggen

Träningsapp för mobilen: profil, tre helkroppspass (A, B, C) från fasta regler, loggning med vilotimer, automatisk viktökning, historik, kroppsviktskurva, skivräknare och synk mellan enheter via Firebase.

## 1. Lägg upp på GitHub Pages

1. Skapa ett nytt repo, till exempel `gymloggen`.
2. Ladda upp alla filer: `index.html`, `sw.js`, `manifest.webmanifest`, `icon.svg`, `icon-192.png`, `icon-512.png`.
3. Settings → Pages → Branch: `main`, mapp `/ (root)` → Save.
4. Öppna `https://DITTKONTO.github.io/gymloggen/` i mobilen och välj **Lägg till på startskärmen**.

Appen fungerar direkt, utan synk. Data sparas då bara i telefonen.

## 2. Slå på synk (Firebase)

Du kan använda samma Firebase-projekt som Tavlan. Gymloggen sparar i en egen samling, `gymloggen`.

1. Firebase-konsolen → Projektinställningar → **Dina appar** → Lägg till webbapp (`</>`). Kopiera `firebaseConfig`.
2. **Authentication** → Sign-in method → aktivera **Google**.
3. **Authentication** → Settings → **Authorized domains** → lägg till `DITTKONTO.github.io`.
4. **Firestore Database** → Rules. Lägg till det här blocket inuti `match /databases/{database}/documents { ... }`. Ersätt inte Tavlans regler, lägg det bredvid:

```
match /gymloggen/{uid} {
  allow read, write: if request.auth != null && request.auth.uid == uid;
}
```

5. Klistra in `firebaseConfig`:
   - **Enklast:** i appen under Profil → Synk mellan enheter. Gör det på varje enhet.
   - **Eller en gång för alla:** ersätt `const FIREBASE_CONFIG = null;` högst upp i skriptet i `index.html` med ditt objekt, till exempel `const FIREBASE_CONFIG = { apiKey: "...", authDomain: "...", projectId: "...", appId: "..." };`

   Firebase-nyckeln är inte hemlig. Det är reglerna ovan som skyddar datan.
6. Logga in med Google i appen. Använd samma konto på alla enheter.

## Så fungerar programreglerna

- **Tre helkroppspass** som roterar A → B → C. Varje pass har ben (knä- eller höftdominant), press, drag, en extraövning och bål.
- **Antal övningar:** 45 min = 5, 60 min = 6, 75 min = 7.
- **Repsintervall efter mål:** styrka 5–8 (6–10 från 50 år), muskler 8–12, hälsa 10–15. Extraövningarna har högre reps.
- **Set:** 3 på basövningarna (4 för erfarna), 2–3 på extraövningarna.
- **Övningsval:** bara utrustning du har bockat i. Fria vikter prioriteras om du har valt det. Nybörjare och personer över 60 får enklare varianter först.
- **Startvikt:** ett försiktigt förslag utifrån kroppsvikt, kön, vana och ålder. Justera fritt under Program.
- **Progression:** klarar du övre gränsen i alla set höjs vikten nästa pass (skivstång +2,5 kg, hantlar ett steg, maskin +5 kg). Hamnar du under nedre gränsen tre pass i rad sänks vikten med 10 %.

## Uppdatera appen

Höj `CACHE = 'gymloggen-v1'` i `sw.js` till `v2` och så vidare när du laddar upp ändringar.
