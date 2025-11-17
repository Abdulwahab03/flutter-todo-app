# Copilot instructions for flutter-todo-app

Short, actionable guidance to help AI coding agents be productive in this repo.

- **Project Type**: Flutter mobile app (single-screen ToDo sample). Root entry: `lib/main.dart` -> `Home` (`lib/screens/home.dart`). No external state-management library; state is local to `Home`.

- **Key directories**:
  - `lib/screens/` : app screens (UI pages). `Home` is the primary screen.
  - `lib/widgets/` : reusable widgets. Example: `lib/widgets/todo_item.dart` implements the list tile and delete button UI.
  - `lib/model/` : domain models. `lib/model/todo.dart` defines `ToDo` and the initial `ToDo.todoList()` sample data.
  - `lib/constants/` : app-wide constants, e.g. `lib/constants/colors.dart` for color palette.
  - `assets/images/` : image assets (see `pubspec.yaml` under `assets:`). `assets/images/avatar.jpeg` is referenced from `Home`.

- **Important files & patterns (examples)**:
  - `lib/screens/home.dart`:
    - Holds `todosList = ToDo.todoList()` and `_foundToDo` used for search/filtering.
    - Input is a `TextField` with `_todoController`; new items are created in `_addToDoItem()` using timestamp `DateTime.now().millisecondsSinceEpoch.toString()` as `id`.
    - Deletion uses `_deleteToDoItem(String id)` -> `todosList.removeWhere((item) => item.id == id)`.
    - Toggling done uses `_handleToDoChange(ToDo todo)` which flips `todo.isDone` and calls `setState()`.
  - `lib/widgets/todo_item.dart`:
    - Stateless widget that expects `todo`, `onToDoChanged`, `onDeleteItem` callbacks.
    - UI: leading checkbox icon (toggles), `title` uses `TextDecoration.lineThrough` when `todo.isDone`, trailing delete `IconButton` calls `onDeleteItem(todo.id)`.
  - `lib/model/todo.dart`:
    - `ToDo` fields: `id`, `todoText`, `isDone` (defaults to `false`). Initial seed list provided in `todoList()`.

- **State & Data flow**:
  - Single-source in-memory data lives in `Home` (`todosList`), and is passed down to `ToDoItem` via props and callbacks. No persistence or backend integration is present.
  - Search is implemented locally via `_runFilter(String)` which updates `_foundToDo`.

- **Conventions and expectations for edits**:
  - Keep UI logic local to `Home` unless intentionally refactoring to a global state manager (explain reason and add tests).
  - Preserve nullable annotations consistent with current SDK constraint (`environment.sdk: ">=2.17.3 <3.0.0"` in `pubspec.yaml`). Avoid migrating to null-safety semantics that require SDK >=3 unless you update `pubspec.yaml` and confirm all code compiles.
  - Colors and small UI constants live in `lib/constants/`; prefer adding new constants there.
  - New widgets belong under `lib/widgets/` with PascalCase filenames (e.g., `my_new_widget.dart` → `MyNewWidget`).

- **Build / test / debug commands (PowerShell on Windows)**:
  - Install deps: `flutter pub get`
  - Run on default device/emulator: `flutter run`
  - Run on Windows (desktop): `flutter run -d windows` (if desktop tooling configured)
  - Run tests: `flutter test`
  - Static analysis: `dart analyze` or `flutter analyze`
  - Build APK: `flutter build apk`

- **What to avoid changing lightly**:
  - Android/iOS native config under `android/` and `ios/` unless the PR is explicitly about platform changes — this is a small tutorial app; changing Gradle configs, bundle ids, or signing files can break reproducible tutorial behavior.
  - `pubspec.yaml` SDK constraint without a good reason — many community contributors may expect the app to run on older stable Flutter that matches `sdk` range.

- **Suggested first tasks for a contributor/assistant**:
  - Add persistent storage (e.g., `shared_preferences`) — put code to load/save in `Home` or extract a repository class in `lib/services/`.
  - Add unit/widget tests around adding, deleting, and searching; use `flutter_test` (there is a starter `test/widget_test.dart`).
  - Improve accessibility: add semantics labels to buttons in `todo_item.dart`.

- **How to run and validate a UI change locally**:
  - `flutter pub get; flutter run` then interact with the app: add, delete, toggle, and use search. Confirm `assets/images/avatar.jpeg` loads (missing asset will throw at runtime).

- **Pull request guidance for agents**:
  - Keep changes small and focused (UI, model, or tests). Explain any cross-cutting refactors (state extraction, SDK bump) in the PR description and include the rationale and manual test steps.
  - Run `flutter test` and `dart analyze` locally; include their outputs in the PR if non-trivial fixes were required.

If anything here is unclear or you want me to expand examples (e.g., add a short PR template, CI config, or automated test examples), tell me which area to expand and I will iterate.