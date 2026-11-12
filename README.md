# MinecraftGmail

A [Fabric](https://fabricmc.net/) mod for Minecraft, scaffolded from the official
[fabric-example-mod](https://github.com/FabricMC/fabric-example-mod) template.

| Component  | Version          |
|------------|------------------|
| Minecraft  | 26.3             |
| Fabric Loader | 0.20.1        |
| Fabric Loom | 1.19-SNAPSHOT   |
| Fabric API | 0.165.2+26.3     |
| Gradle     | 9.8.2 (wrapper)  |
| Java       | 25+ (to build)   |

## Prerequisites

- A full **JDK 25** (e.g. Eclipse Temurin, Microsoft OpenJDK). A JRE is not enough —
  Gradle needs `javac` to compile the mod.
- Network access to `services.gradle.org`, `maven.fabricmc.net`, Maven Central,
  and Mojang's servers (Gradle downloads the toolchain, mappings, and Minecraft
  itself on first build).

## Build

```sh
./gradlew build
```

The remapped mod jar lands in `build/libs/`.

Other useful tasks:

```sh
./gradlew runClient   # launch the Minecraft client with the mod
./gradlew runServer   # launch a test server with the mod
./gradlew genSources  # generate decompiled Minecraft sources for your IDE
```

Every push / pull request is also built by the `build` GitHub Actions workflow
(see [.github/workflows/build.yml](.github/workflows/build.yml)).

## Project layout

```text
src/
  main/
    java/io/github/loganwall111/minecraftgmail/
      MinecraftGmailMod.java          # main entrypoint (ModInitializer)
      mixin/MinecraftGmailMixin.java  # example server-side mixin
    resources/
      fabric.mod.json                 # mod metadata, entrypoints, dependencies
      minecraftgmail.mixins.json      # server mixin config
      assets/minecraftgmail/          # icon and other assets
  client/
    java/.../client/
      MinecraftGmailClient.java       # client entrypoint (ClientModInitializer)
      mixin/MinecraftGmailClientMixin.java
    resources/
      minecraftgmail.client.mixins.json
```

## Roadmap

Ideas for where this mod goes next (all TODO):

- Secure config for Google API credentials (OAuth 2.0, stored outside the jar).
- In-game Gmail inbox screen (unread count, subjects, senders).
- Toast/hud notification when new mail arrives.
- Send simple emails from a command or GUI.

## Environment note

The Agent sandbox this was scaffolded in has restricted network access
(only `github.com`, `registry.npmjs.org`, and `pypi.org` are reachable), so
`./gradlew build` **cannot** run there: the Gradle distribution, Loom, and all
Minecraft/Maven dependencies live on unreachable hosts, and only JRE-level
Java runtimes (21.0.8 and 25.0.2, via the `jdk4py` PyPI package) could be
installed — no `javac`. Both runtimes were verified to execute Java bytecode.
Run the build on a machine (or CI runner) with a full JDK 25 and open internet.
