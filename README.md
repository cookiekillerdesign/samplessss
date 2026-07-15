# BirthdayPulse — MVP (Flutter, Android)

Реализовано по ТЗ (раздел 3.1 MVP):
- Контакты: добавление вручную + импорт из системной адресной книги (только с датой рождения)
- Многоступенчатые напоминания: за 7 дней / за 1 день / в день (дефолт), офлайн, точные будильники
- Экран "Сегодня/Скоро": группировка Сегодня / Неделя / Месяц / Позже
- Календарь месячного вида с маркерами событий
- Material Design 3 + Dynamic Color (Material You)
- Локальная SQLite-БД (офлайн-first), без бэкенда

## Разворачивание проекта

Так как в этой директории только исходники `lib/` — нативные Android/iOS обёртки нужно сгенерировать локально:

```bash
flutter create --org com.birthdaypulse --project-name birthday_pulse .
# подтвердить перезапись pubspec.yaml НЕ нужно — используйте флаг ниже, если попросит:
# flutter create --overwrite ...
```

Затем скопируйте содержимое папки `lib/` из архива поверх сгенерированного `lib/` и замените `pubspec.yaml` на приложенный.

```bash
flutter pub get
```

## Обязательные правки для Android (`android/app/src/main/AndroidManifest.xml`)

Добавить внутрь `<manifest>`:

```xml
<uses-permission android:name="android.permission.READ_CONTACTS"/>
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
<uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM"/>
<uses-permission android:name="android.permission.USE_EXACT_ALARM"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
```

Внутри `<application>` (нужно для перепланирования уведомлений после перезагрузки устройства, flutter_local_notifications делает это автоматически при наличии разрешения выше).

Минимальный `minSdkVersion` в `android/app/build.gradle`: **23** (требование flutter_local_notifications).

## Запуск

```bash
flutter run -d <android-device-id>
```

## Что НЕ входит в этот MVP-срез (см. полный ТЗ, разделы 3.2+)

- AI-генератор поздравлений и подсказки подарков
- Home screen widget (нативный WidgetKit/Glance код)
- Синхронизация между устройствами / облако
- Wishlist, трекер подарков, годовой рекап
- Подписки (RevenueCat)

Эти блоки — следующий этап (v1.5–2.0) поверх текущей Clean Architecture (папки `features/*` уже разделены под расширение).
