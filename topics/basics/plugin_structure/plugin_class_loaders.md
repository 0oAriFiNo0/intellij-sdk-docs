[//]: # (title: Class Loaders)

<!-- Copyright 2000-2022 JetBrains s.r.o. and other contributors. Use of this source code is governed by the Apache 2.0 license that can be found in the LICENSE file. -->

A separate class loader is used to load the classes of each plugin.
This allows each plugin to use a different library version, even if the same library is used by the IDE itself or by another plugin.

## Overriding IDE Dependencies

Gradle 7 introduced `implementation` scope, replacing `compile` scope.
For this setup to use project defined dependency instead of the bundled IDE version, please add this snippet to your <path>build.gradle.kts</path>:

```kotlin
configurations {
  all {
    resolutionStrategy.sortArtifacts(ResolutionStrategy.SortOrder.DEPENDENCY_FIRST)
  }
}
```

## Classes from Plugin Dependencies

By default, the main IDE class loader loads classes that are not found in the plugin class loader.
However, in the <path>plugin.xml</path> file, you may use the `<depends>` element to specify that a [plugin depends](plugin_dependencies.md) on one or more other plugins.
In this case, the class loaders of those plugins will be used for classes not found in the current plugin.
This allows a plugin to reference classes from other plugins.

## Using ServiceLoader

Some libraries use [`ServiceLoader`](https://docs.oracle.com/javase/8/docs/api/index.html?java/util/ServiceLoader.html) to detect and load implementations.
For this to work in a plugin, the context class loader must be set to the plugin's classloader and restored afterwards with original one around initialization code:

```java
    ClassLoader current = Thread.currentThread().getContextClassLoader();
    try {
        Thread.currentThread().setContextClassLoader(this.getClass().getClassLoader());
        // code working with ServiceLoader here
    } finally {
        Thread.currentThread().setContextClassLoader(current);
    }
```
