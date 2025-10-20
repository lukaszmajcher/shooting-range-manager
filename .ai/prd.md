# Dokument wymagań produktu (PRD) - ShootingRangeManager

## 1. Przegląd produktu
Aplikacja ShootingRangeManager automatyzuje proces rezerwacji osi strzelniczych, zastępując tradycyjne, papierowe formy zarządzania grafikami. Umożliwia właścicielom łatwe konfigurowanie infrastruktury i godzin otwarcia, a użytkownikom szybki podgląd dostępności oraz rezerwację zarówno bez konta, jak i po zalogowaniu.

## 2. Problem użytkownika
Właściciele strzelnic tracą wiele czasu na obsługę telefoniczną lub osobistą, prowadząc papierowe grafiki. Klienci nie mogą samodzielnie sprawdzić dostępności ani zarezerwować terminu online, co skutkuje dużą liczbą połączeń telefonicznych i opóźnieniami.

## 3. Wymagania funkcjonalne

Panel właściciela:
- zarządzanie infrastrukturą: do 30 osi strzeleckich z nazwami, parametrami technicznymi i statusem aktywności
- zarządzanie czasem: definiowanie godzin otwarcia dla każdego dnia, blokowanie terminów na wydarzenia specjalne
- obsługa rezerwacji: przegląd i ręczne dodawanie rezerwacji telefonicznych, edycja i anulowanie wszystkich rezerwacji
- analityka: dashboard z metrykami obłożenia, źródłami rezerwacji (online vs telefon), eksport danych do CSV

Panel użytkownika:
- podgląd dostępności: widok tygodniowy z filtrowaniem po osiach lub parametrach osi np. długość, tryby: wszystkie osie lub konkretna oś
- rezerwacje gości: pojedyncze lub grupowe (do 10 osi), formularz kontaktowy (imię, telefon, email), potwierdzenie emailem w ciągu 30 minut
- konta użytkowników: rejestracja/logowanie, podgląd historii rezerwacji i aktywnych (do 3 jednocześnie), szybka rezerwacja z zapisanymi danymi, anulowanie min. 24h przed terminem

## 4. Granice produktu
- bez obsługi płatności (rozliczenia na miejscu)
- brak integracji z OAuth (tylko własny system logowania)
- pojedyncza instalacja dla jednej strzelnicy
- prosty model uprawnień: dwie role (właściciel, użytkownik)
- brak optimistic locking (akceptowane ryzyko konfliktów)

## 5. Historyjki użytkowników

- ID: US-001
  Tytuł: Rezerwacja jako gość
  Opis: Klient sprawdza dostępność i rezerwuje oś bez zakładania konta, aby uniknąć dzwonienia.
  Kryteria akceptacji:
  - widok kalendarza prezentuje wolne osie na wybrany tydzień
  - formularz wymaga imienia, telefonu i emaila
  - możliwa rezerwacja jednek osi jednocześnie
  - system wysyła potwierdzenie mailem z prośbą o potwierdzenie w ciągu 30 minut
  - rezerwacja pojawia się w dashboardzie właściciela

- ID: US-002
  Tytuł: Rejestracja i logowanie użytkownika
  Opis: Użytkownik zakłada konto i loguje się, aby zarządzać rezerwacjami.
  Kryteria akceptacji:
  - formularz rejestracji waliduje unikalny email i bezpieczne hasło
  - logowanie wymaga emaila i hasła
  - sesja trwa do wylogowania lub 30 dni
  - tylko zalogowani użytkownicy widzą historię i aktywne rezerwacje

- ID: US-003
  Tytuł: Rezerwacja jako zarejestrowany użytkownik
  Opis: Stały klient szybko rezerwuje oś z użyciem zapisanych danych.
  Kryteria akceptacji:
  - dane (imię, telefon, email) są automatycznie uzupełniane
  - użytkownik może mieć maks. 3 aktywne rezerwacje
  - próba 4. rezerwacji jest zablokowana z komunikatem
  - anulowanie możliwe min. 24h przed terminem

- ID: US-004
  Tytuł: Rezerwacja jako zarejestrowany użytkownik dla grupy
  Opis: Klient rezerwuje dla grupy do 10 osi w jednym terminie.
  Kryteria akceptacji:
  - formularz umożliwia zaznaczenie maks. 10 osi
  - wszystkie osie muszą być dostępne w wybranym terminie
  - potwierdzenie rezerwacji jest wysyłane na email uzytkownika
  - rezerwacja pojawia się w dashboardzie właściciela do akceptacji
  - po akceptacji przez właściciela wysyłany jest email z potwierdzeniem rezerwacji

- ID: US-005
  Tytuł: Anulowanie rezerwacji
  Opis: Użytkownik (zalogowany) może anulować rezerwację online.
  Kryteria akceptacji:
  - anulowanie dostępne tylko do 24h przed terminem
  - system aktualizuje dostępność w kalendarzu natychmiast
  - potwierdzenie anulowania wysyłane mailem
  - właściciel widzi powód anulowania w dashboardzie

- ID: US-006
  Tytuł: Zarządzanie grafikiem przez właściciela
  Opis: Właściciel przegląda i modyfikuje rezerwacje w panelu admina.
  Kryteria akceptacji:
  - widok listy wszystkich rezerwacji z danymi kontaktowymi
  - możliwość ręcznego dodania rezerwacji telefonicznej
  - możliwość anulowania lub edycji istniejącej rezerwacji

- ID: US-007
  Tytuł: Blokowanie terminów na wydarzenia specjalne
  Opis: Właściciel blokuje osie na wybrane terminy.
  Kryteria akceptacji:
  - właściciel wybiera datę i zakres godzin do zablokowania
  - wybrane osie są oznaczane jako niedostępne
  - użytkownicy nie mogą rezerwować zablokowanych terminów

- ID: US-008
  Tytuł: Podgląd analityki i eksport danych
  Opis: Właściciel analizuje obłożenie i eksportuje dane.
  Kryteria akceptacji:
  - dashboard wyświetla obłożenie dzienne, tygodniowe i kanały rezerwacji
  - możliwy eksport CSV wszystkich rezerwacji w zadanym okresie

## 6. Metryki sukcesu

Kryteria biznesowe:
- redukcja telefonów o dostępność o min. 50% (pomiar: udział rezerwacji online vs telefon)
- redukcja telefonów o anulowanie o min. 70% (pomiar: stosunek anulowań online do telefonicznych)
- 60% rezerwacji realizowanych online w ciągu 3 miesięcy od wdrożenia

Kryteria techniczne:
- dostępność systemu 99% w godzinach pracy
- obsługa do 100 równoczesnych użytkowników
- czas odpowiedzi < 2 sekundy dla operacji podglądu i rezerwacji
