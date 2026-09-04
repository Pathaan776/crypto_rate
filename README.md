# 📈 CryptoRate (Flutter)

[![CI](https://github.com/Pathaan776/crypto_rate/actions/workflows/ci.yml/badge.svg)](https://github.com/Pathaan776/crypto_rate/actions/workflows/ci.yml)

A Flutter sample application built with Clean Architecture, featuring a responsive UI, dark theme, REST API integration (Dio), and simulated live price updates.

---

## ✨ Highlights

- Clean Architecture (data / domain / presentation) with a feature-first structure
- Dark theme with custom typography
- Responsive layout (mobile + tablet/desktop friendly)
- Crypto market list with:
  - Search functionality
  - Watchlist tabs
  - Pull-to-refresh
- Reorderable watchlist screen (drag & drop support)
- Data source: CoinGecko REST API

### 🔄 Live Updates Approach

- Polling-based price stream (updates every few seconds)
- Automatic fallback to demo data if the network or DNS fails

---

## 🌐 API Used

**Market list**

```http
GET https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=10&page=1&sparkline=false
```

**Live prices**

CoinGecko does not provide WebSocket support, so the app polls this endpoint instead:

```http
GET https://api.coingecko.com/api/v3/simple/price?ids=...&vs_currencies=usd&include_24hr_change=true
```

---

## 🏗 Project Structure (Clean Architecture)

```txt
lib/
  app/
    app.dart
  core/
    theme/
      app_theme.dart
    utils/
      formatters.dart
  features/
    market/
      data/
        datasources/
          coincap_market_api.dart        // CoinGeckoMarketApi (REST)
          coincap_market_ws.dart         // WebSocket source (unused)
          demo_market_data.dart          // Fallback data
        models/
          crypto_asset_model.dart
        repositories/
          market_repository_impl.dart    // Polling stream + fallback
      domain/
        entities/
          crypto_asset.dart
        repositories/
          market_repository.dart
        usecases/
          get_top_assets.dart
          watch_prices.dart
      presentation/
        cubit/
          market_cubit.dart
          market_state.dart
        pages/
          market_page.dart
          reorder_watchlist_page.dart
        widgets/
          asset_tile.dart
          market_top_ticker.dart
```

---

## ⚡ How Live Updates Work

- CoinGecko's market data is REST-only and does not expose WebSocket endpoints.
- The app periodically calls the `simple/price` endpoint to simulate real-time updates.
- If a network or DNS issue occurs (for example, emulator connectivity problems or blocked DNS), the app automatically switches to demo data.

This keeps the UI functional even without network access.

---

## 🚀 Getting Started

### Requirements

- Flutter 3.44.7 (stable) or newer
- Dart SDK `^3.10.3`

### Run locally

```bash
flutter pub get
flutter run
```

### Quality checks

```bash
dart format .
flutter analyze
flutter test
```

---

## 🤖 Continuous Integration

Every push and pull request to `main` runs [`.github/workflows/ci.yml`](.github/workflows/ci.yml), which:

1. Checks formatting with `dart format`
2. Runs `flutter analyze`
3. Runs `flutter test --coverage` and uploads `lcov.info`
4. Builds a release APK and a web bundle, uploading both as artifacts

Dependabot ([`.github/dependabot.yml`](.github/dependabot.yml)) opens weekly PRs for `pub` and GitHub Actions updates.

---

## 📝 Notes

- Data flows in one direction: **Data Sources → Repository → Use Cases → Cubit → UI**
- To use a real WebSocket or socket.io backend, replace the polling implementation in
  `MarketRepository.watchPrices()` with a streaming one.

---

## 📄 License

Built for demonstration purposes.
