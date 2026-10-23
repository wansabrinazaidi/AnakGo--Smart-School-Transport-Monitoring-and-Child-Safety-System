# AnakGo – pickup / arrived with live tracking

Drivers tap **Picked up** and **Arrived** for each child. While a trip is running the driver's phone shares its GPS location, and parents watch the van move on a live map.

## Quick demo (no setup, one computer)
1. Open the folder in VS Code > right-click `index.html` > **Open with Live Server**.
2. Tab 1: Driver login `driver1` / `demo123`. Choose Home → School and tap **Start trip**. Allow location access.
3. Tab 2 (same browser): Parent login `parent1` / `demo123` > **Live tracking**.
4. In tab 1, tap **Picked up** for Aiman, then **Arrived** (or **All arrived**). Tab 2 updates by itself.

Without Firebase, the app only syncs between tabs of the same browser. The header shows **⚪ Demo mode**.

## Real phones (Firebase, free)
1. Go to https://console.firebase.google.com and create a project (Google Analytics is not needed).
2. **Build > Realtime Database > Create database**. Choose **Singapore (asia-southeast1)** and **Start in test mode**.
3. **Project settings > General > Your apps > Web (</>)**. Register an app and copy the `firebaseConfig` object.
4. Paste it into `firebase-config.js` as `window.ANAKGO_FIREBASE = { ... }`. Make sure `databaseURL` is included.
5. Reload. The header should show **🟢 Live sync**.
6. Phones need **HTTPS** for GPS. `http://192.168.x.x` from Live Server will NOT get location. Host the folder on HTTPS, for example:
   - Firebase Hosting: `npm i -g firebase-tools`, `firebase login`, `firebase init hosting` (public directory `.`), `firebase deploy`
   - or drag the folder onto https://app.netlify.com/drop

## How a trip works
- **Start trip** turns on live location. Parents see "Awaiting pickup".
- **✅ Picked up** for each child: the parent sees "Picked up 06:52" and gets a notification.
- **🏁 Arrived** for each child at their drop-off. **All arrived** marks everyone still on board at once (e.g. at school).
- **Finish trip** needs everyone marked Arrived plus a physical vehicle check. Location sharing then stops.

## Limitations (prototype)
- The driver must keep AnakGo open with the screen on. Phones pause GPS for background browser tabs. Parents see ⚠️ when no update has arrived for 2 minutes.
- Test-mode Firebase rules let anyone with the database URL read and write. They also expire after 30 days. Do not use this with real children's locations until proper login (Firebase Authentication) and security rules are added.
- Logins are demo accounts stored in the app code. Payments are records only.
