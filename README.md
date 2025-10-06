# DuplicateFinder

DuplicateFinder is a [Playnite](https://playnite.link/) extension that offers a simple way to find duplicate games in your library.

In times of free Epic Games Store games, GOG giveaways, Itch.io collaborative bundles, and monthly Amazon Prime Gaming rewards, it's easier than ever to end up with a game library bloated with duplicates.

DuplicateFinder adds a sidebar view to your Playnite UI, allowing you to search for duplicate games and making it easier to find games you might want to hide or remove from your library.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Disclaimer](#disclaimer)
- [Contribute](#contribute)
- [Third-Party Libraries](#third-party-libraries)
- [License](#license)

## Features
- Identifies duplicate games in your library based on the game's name.
- Allows you to include or exclude hidden games from the search.
- Displays results in a detailed, ordered view showing the name, platform and hidden status of each game.
- Includes a '_Check for similarity_' option to search for **potentially** duplicate games with non-matching names.
- Offers a customizable _Tolerance_ setting to fine-tune the '_Check for similarity_' results.

## Installation

### From Playnite Add-ons Browser (Recommended)

1. Open Playnite
2. Press `F9` or go to `Add-ons` > `Browse`
3. Search for "DuplicateFinder"
4. Click `Install`
5. Restart Playnite

### Manual Installation

1. Download the latest release from the [Releases page](https://github.com/ap0ught/Playnite-DuplicateFinder/releases)
2. Extract the archive
3. Copy the folder to your Playnite extensions directory:
   - Default location: `%AppData%\Playnite\Extensions\`
4. Restart Playnite

## Usage

1. After installation, you'll find a new **DuplicateFinder** section in Playnite's sidebar
2. Configure your search options:
   - **Include hidden games**: Check this to include hidden games in the search
   - **Check for similarity**: Enable fuzzy matching to find games with similar (but not identical) names
   - **Tolerance**: Adjust the sensitivity of similarity matching (lower = stricter matching)
3. Click the **Find Duplicates** button
4. Review the results in the sidebar
5. Click on any game in the results to view it in your library

### Tips

- Start with exact matching (uncheck "Check for similarity") to find obvious duplicates
- When using similarity matching, begin with a low tolerance value and increase gradually
- Review each result carefully before hiding or removing games, especially when using similarity matching
- Use the results to identify games you want to hide or remove manually from your library

## Disclaimer
- When using '_Check for similarity_', false positives for sequels or similarly named games are expected (e.g. _Far Cry 3_ and _Far Cry 4_, or _Feud_ and _Fez_). While this feature is useful for identifying potential duplicates, always review the results to ensure accuracy.
- DuplicateFinder was built with full awareness of the existence of [DuplicateHider](https://github.com/felixkmh/DuplicateHider) by **felixkmh**. However, DuplicateFinder was created with simplicity in mind, offering fewer features and less automation, but with greater ease of use and clarity. It provides information so users can review and decide what to do on a per-game basis. It is also designed to be easier to maintain and keep compatible with every version of Playnite.

## Contribute

Contributions are welcome! If you feel DuplicateFinder is lacking an important feature or something needs fixing, please:

1. Open an issue to discuss your ideas before starting work
2. Check the [README.dev.md](README.dev.md) file for detailed developer documentation
3. Follow the code style guidelines outlined in the developer documentation
4. Submit a pull request with your changes

For detailed development setup, architecture information, and contribution guidelines, please see [README.dev.md](README.dev.md).

## Third-Party Libraries

This project uses the following third-party libraries:

- [Fastenshtein](https://github.com/DanHarltey/Fastenshtein) is licensed under the [MIT License](https://github.com/DanHarltey/Fastenshtein/blob/master/LICENSE) - Copyright (c) 2017 DanHartley
- [PlayniteSDK](https://playnite.link/) is licensed under the [MIT License](https://github.com/JosefNemec/Playnite/blob/master/LICENSE.md) - Copyright (c) 2020 Josef Nemec

## License

This project is licensed under the MIT License. See [LICENSE.md](LICENSE.md) for details.

Copyright (c) 2024 Pablo García de los Salmones Gómez
