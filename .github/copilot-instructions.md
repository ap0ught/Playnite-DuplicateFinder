# Copilot Instructions for DuplicateFinder

## Project Overview

DuplicateFinder is a Playnite extension that helps users identify duplicate games in their library. The project is built in C# targeting .NET Framework 4.6.2 and integrates with the Playnite SDK.

## Key Technologies

- **Language**: C# (.NET Framework 4.6.2)
- **Build System**: MSBuild
- **UI Framework**: WPF (Windows Presentation Foundation)
- **Architecture**: MVVM (Model-View-ViewModel) pattern
- **Dependencies**:
  - PlayniteSDK 6.11.0
  - Fastenshtein 1.0.10 (Levenshtein distance algorithm)

## Code Style Guidelines

**IMPORTANT**: This project follows specific C# coding conventions. Always adhere to these rules:

### Naming Conventions

- **Private fields**: Use camelCase with a preceding underscore (e.g., `_sidebarView`, `_includeHiddenGames`)
- **Public and private properties**: Use PascalCase (e.g., `FoundDuplicates`, `CheckSimilarity`)
- **Methods**: Use PascalCase for both public and private methods (e.g., `FindDuplicates()`, `ExecuteDuplicateFinder()`)
- **Do not expose public fields** - use properties instead

### Formatting Rules

- **Indentation**: Use 4 spaces, NOT tabs
- **Braces**: Place opening curly brace `{` on a new line (Allman style)
- **Empty lines**: Add an empty line between the end of a code block `}` and any additional expression
- **Always use braces**: Encapsulate code bodies after `if`, `for`, `foreach`, `while`, etc. with curly braces, even for single-line statements

### Correct Example

```csharp
if (includeHiddenGames)
{
    searchList = PlayniteApi.Database.Games;
}
else
{
    searchList = PlayniteApi.Database.Games.Where(game => !game.Hidden);
}

results = FindDuplicates(searchList);
```

### Incorrect Example

```csharp
if (includeHiddenGames)
    searchList = PlayniteApi.Database.Games;
else
    searchList = PlayniteApi.Database.Games.Where(game => !game.Hidden);
results = FindDuplicates(searchList);
```

## Project Architecture

### Main Components

1. **DuplicateFinder.cs**: Main plugin class inheriting from `GenericPlugin`
   - Manages plugin lifecycle and integrations with Playnite
   - Contains duplicate detection logic
   - Uses Levenshtein distance for similarity checking

2. **DuplicateFinderSidebarView.xaml/.cs**: WPF UserControl for UI
   - Displays the sidebar interface in Playnite
   - Bound to ViewModel using data binding

3. **DuplicateFinderSidebarViewModel.cs**: ViewModel implementation
   - Inherits from `ObservableObject`
   - Manages UI state and communicates with the plugin
   - Uses events to request duplicate finding operations

### Design Patterns

- **MVVM**: Strict separation between View (XAML), ViewModel, and Model (plugin logic)
- **Event-driven**: ViewModel communicates with plugin through events (e.g., `RequestFindDuplicates`)
- **Singleton pattern**: Uses Playnite SDK's API instance (`Playnite.SDK.API.Instance`)

## Building the Project

```bash
# Restore NuGet packages
dotnet restore DuplicateFinder.sln

# Build the project
dotnet build DuplicateFinder.sln --configuration Release
```

## Testing Considerations

- This is a Playnite plugin that requires the Playnite application to run
- Manual testing is performed within Playnite
- No automated test infrastructure currently exists

## Key Features

1. **Exact duplicate detection**: Finds games with identical names
2. **Similarity detection**: Uses Levenshtein distance algorithm to find similar game names
3. **Configurable tolerance**: Allows fine-tuning similarity search sensitivity
4. **Hidden games filter**: Option to include or exclude hidden games from search

## Important Notes

- The plugin uses the Playnite SDK API accessed through `Playnite.SDK.API.Instance`
- Localization is supported through XAML resource dictionaries (en_US, es_ES)
- The plugin provides a sidebar view that integrates into the Playnite UI
- Performance consideration: Similarity checking uses `Fastenshtein.Levenshtein` with the reusable instance pattern for efficiency

## Contribution Guidelines

Before making changes:
- Open an issue to discuss the feature or fix
- Ensure changes maintain compatibility with Playnite SDK
- Follow the coding style guidelines strictly
- Keep the plugin simple and focused on core duplicate-finding functionality

## License

MIT License - Copyright (c) 2024 Pablo García de los Salmones Gómez
