# Ajolotito hambriento – notificaciones locales con TDD

Proyecto del *Manual TDD Ajolotito Notificaciones*. El código de `lib/`, `test/` y
`pubspec.yaml` es el del manual, tal cual (incluida su carpeta `lib/infra`).

## 1. Preparar (una sola vez)

El zip no trae las carpetas de plataforma ni las dependencias, para que pese poco.
Dentro de la carpeta `ajolotito`:

```
flutter create . --project-name ajolotito
flutter pub get
```

> ⚠️ `flutter create .` agrega **test/widget_test.dart**, que busca `MyApp` (no existe aquí)
> y hace fallar `flutter test`. **Bórralo.**
> El pubspec pide Dart `^3.8.0`: si `pub get` se queja, actualiza Flutter (`flutter upgrade`).

## 2. Configuración de Android (Ronda 1 del manual)

`android/app/src/main/AndroidManifest.xml`, dentro de `<manifest>` (fuera de `<application>`):

```xml
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

Dentro de `<application>`:

```xml
<receiver android:exported="false"
    android:name="com.dexterous.flutterlocalnotifications.ScheduledNotificationReceiver" />
<receiver android:exported="false"
    android:name="com.dexterous.flutterlocalnotifications.ScheduledNotificationBootReceiver">
  <intent-filter>
    <action android:name="android.intent.action.BOOT_COMPLETED"/>
    <action android:name="android.intent.action.MY_PACKAGE_REPLACED"/>
    <action android:name="android.intent.action.QUICKBOOT_POWERON" />
    <action android:name="com.htc.intent.action.QUICKBOOT_POWERON"/>
  </intent-filter>
</receiver>
```

`android/app/build.gradle.kts` (agrégalo a los bloques que ya existen):

```kotlin
android {
    compileOptions {
        isCoreLibraryDesugaringEnabled = true
    }
}
dependencies {
    coreLibraryDesugaring("com.android.tools:desugar_jdk_libs:2.1.4")
}
```

> Si Android se queja de versiones (desugar, Java, compileSdk), revisa el README del paquete
> `flutter_local_notifications` para la versión que se instaló, como dice el manual.

## 3. iOS

En `ios/Runner/AppDelegate.swift`, dentro de `application(_:didFinishLaunchingWithOptions:)`,
antes del `return`:

```swift
UNUserNotificationCenter.current().delegate = self as? UNUserNotificationCenterDelegate
```

## 4. Ejecutar

```
flutter test
flutter run
```

## 5. Rondas TDD y capturas

Ver **GUIA_RONDAS.md**.
