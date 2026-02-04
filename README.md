# 4x Game

> [!NOTE]
> This project is in very early development. When finished, this description might be true

This is a realtime fantasy 4x game inspired by games such as Age of Wonders and Kohan. It features procedural map generation,
in-map realtime battles, and a robust character interaction and progression system.

## IMPORTANT

TODO:
> [!WARNING]
> This is a template. This section contains actions you must perform to use this template properly
>
> - Update README.md (this file)
>   - fill in each section with a TODO
>   - remove each TODO and its associated [!WARNING] block
>
> - Update [.github/workflows/main.yml](.github/workflows/main.yml)
>   - Uncomment the `on: push` line to cause push-based CI builds to happen
>
> - Update [.github/actions/benchmark/action.yml](.github/actions/benchmark/action.yml)
>   - Where `"--project", "<path to benchmark project>"` is specified, update to use the actual project name
>
> - Remove this section

## Features

A list of features this project provides.

[See the detailed plan](./docs/plans/4x-game.md)

- Procedural map generation
  - Tile-based using wave function collapse

- Battles happen on the map, rather than moving to a separate battle map, encouraging interaction between
  an ongoing battle and the wider world
  - Armies use a formation system - set your infantry in the centre and cavalry on the flanks
  - Destruction caused by war affects the map
  - Relief forces can join the battle at the right moment and save the day

- Character system
  - The game is built around characters
  - Characters can lead armies or govern cities, lending bonuses
  - Characters can gain traits as they perform actions, giving them bonuses or flaws
  - Characters can lead factions and push their own agenda
  - Characters can join your council, giving empire-wide bonuses and offering advice

- Faction system
  - Factions are led by characters and defined by the agendas those characters have
  - Factions can be of several types
    - National factions are the player controlled faction and peers
    - Racial factions are tied to a race
      - Will react to actions that affect that race, such as pogroms or discrimmination
      - May try to convert or attack members of other racial factions
    - Religious factions are tied to a belief system
      - Will react to actions that affect that religion, such as pogroms or discrimmination
      - May try to convert or attack members of other religious factions
    - Political factions represent sections of society such as merchants, military and religion
    - Economic factions represent different levels of wealth such as peasantry, landowners and nobles
    - Cultural factions represent a combinaton of cultural groups.
      - May generate culture within an empire
      - May also generate cultural tensions as different groups clash

- Resource system
  - Physical resources
    - Food
      - Access to multiple sources of food give additional happiness bonuses
    - Wood
    - Stone
    - Metals
    - Gold / money
    - Luxuries
  - Magical resources
  - Cultural resources

- City management system

- Diplomacy system

- Trade system

## Roadmap

TODO:
> [!WARNING]
> This is a template. Update the roadmap to describe the work in development, with links to
> projects and issues

## Prerequisites

TODO:
> [!WARNING]
> This is a template. Update the prerequites to describe what someone needs in order to use this
> project.
>
> This should be a comprehensive but concise list. It should list, for example, the version of .NET
> that is required and provide a link, but it should not document in detail the installation process
> as the dotnet project itself already does that.

- [.NET SDK](https://dotnet.microsoft.com/download) version 10.0
- Any other dependencies or tools required

## Installation

TODO:
> [!WARNING]
> This is a template. Update the installation instructions to describe what a user must do in order to
> set up this project for use
>
> This should build upon the prerequisites.
>
> For projects with no installation, instruct the user to clone the repo, build and run tests
>
> For library projects with a nuget package, instruct the user to add the package reference. Link to the
> nuget package page which allows the user to copy the command or package syntax for different package
> managers and package versions
>
> For application projects and other projects with a release, instruct the user to download the release and
> link to the releases page.

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   cd your-repo-name
   ```

2. Restore NuGet packages:

   ```bash
   dotnet restore
   ```

3. Build the project:

   ```bash
   dotnet build
   ```

## Usage

TODO:
> [!WARNING]
> This is a template. Update to describe how a user can use this project

Provide examples of how to use the application or library. Include code snippets if applicable.

### Running the Application

TODO:
> [!WARNING]
> This is a template. Update to describe how a user can run the application
>
> Remove this section if this is not an application project

   ```bash
   dotnet run
   ```

## Configuration

TODO:
> [!WARNING]
> This is a template. Update to describe how a user can configure the application or library
>
> Include code snippets
>
> Remove this section if there is no configuration

If applicable, describe any configuration options or environment variables.

## Contributing

TODO:
> [!WARNING]
> This is a template. Update to describe how a user can contribute to this library
>
> Describe the procedures for
>
> - raising issues
>   - bugs
>   - feature requests / suggestions
> - forking / cloning the repo
> - building
> - running tests
>   - unit tests
>   - integration tests
>   - mutation / property tests
>   - benchmarks
> - updating attributions
> - running verification checks
> - submitting PRs

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

TODO:
> [!WARNING]
> This is a template. Update if using a different license

This project is licensed under the [MIT License](LICENSE) - see the [LICENSE](LICENSE) file for details.

## Contact

TODO:
> [!WARNING]
> This is a template. Update to match this repo

- Project Link: [https://github.com/your-username/your-repo-name](https://github.com/your-username/your-repo-name)
- Email: <your-email@example.com>
