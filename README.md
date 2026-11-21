# Шаховий Онлайн-Турнір (PWA + Firebase)

## 1. Firebase (5 хвилин)
1. https://console.firebase.google.com → **Add project**.
2. **Build → Authentication → Sign-in method → Anonymous → Enable**.
3. **Build → Firestore Database → Create database** (production mode).
4. Вкладка **Rules** → вставте та натисніть Publish:
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/public/data/tournament/{docId} {
      allow read, write: if request.auth != null;
    }
  }
}
```
5. **Project settings → Your apps → Web (</>)** → скопіюйте `firebaseConfig` у файл `firebase-config.js`.
6. (Необов'язково) задайте `adminPin` у `firebase-config.js`.
   Це лише захист інтерфейсу, не серверний: для турніру з друзями достатньо.

## 2. GitHub Pages
1. Створіть репозиторій, завантажте **весь вміст папки** (index.html, manifest.json, sw.js, firebase-config.js, icons/).
2. Settings → Pages → Branch `main` / root → Save.
3. Адреса: `https://<user>.github.io/<repo>/`
4. Firebase → Authentication → Settings → **Authorized domains** → додайте `<user>.github.io`.

## 3. APK через PWABuilder
1. https://www.pwabuilder.com → вставте адресу GitHub Pages → Start.
2. Перевірте, що Manifest і Service Worker зелені → **Package for stores → Android → Generate**.
3. Завантажте .apk (для встановлення) або .aab (для Google Play).

## Як проходить турнір
1. На вході: вибір мови (УКР / ENG), назва та випадаючий список гравців: Папуля, Мамуля, Сева, Катя, Оксана, Оді, Гість.
2. **Папуля завжди адміністратор** (і водночас учасник). Якщо в `firebase-config.js` задано `adminPin`, для входу Папулі потрібен PIN.
3. «Гість» вводить власне ім'я. Папуля може створити нового постійного гравця в адмін-панелі («Створити нового гравця»): він з'явиться у списку на всіх пристроях.
4. Коли всі увійшли, Папуля натискає «Сформувати пари / Старт»: пари випадкові, кожен грає зі свого пристрою.
5. Після завершення турніру налаштування (час гри, кількість ігор, очікування) повертаються до початкових, а під назвою залишається напис «🏆 Чемпіон: Ім'я» (чемпіон останнього турніру). Новий турнір можна запустити одразу кнопкою «Сформувати пари / Старт».
6. Папуля бачить кнопку **«📊 Статистика перемог»** в адмін-панелі: перемоги в турнірах, 2-ге й 3-тє місця, виграні матчі та партії, історія турнірів. Статистика зберігається в Firebase і не скидається кнопкою «Скинути турнір».
7. Кнопка зміни дизайну (зверху зліва на першій сторінці, а також у шапці): **Minecraft** (за замовчуванням, з піксельними фігурами) і **Класика**. Вибір запам'ятовується на пристрої. Шрифти Minecraft-теми підвантажуються з Google Fonts (потрібен інтернет, інакше буде запасний шрифт).
8. Під смартфон: дошка підлаштовується під висоту екрана (вертикально й горизонтально), великі зони дотику, екран не гасне під час партії.
