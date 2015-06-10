---
layout: general
title: 17. Commenter
---

A commenter allows user to comment the code at the cursor or selected code automatically via *⌘/*.

### 17.1. Define a commenter

```java
package com.simpleplugin;

import com.intellij.lang.Commenter;
import org.jetbrains.annotations.Nullable;

public class SimpleCommenter implements Commenter {
    @Nullable
    @Override
    public String getLineCommentPrefix() {
        return "#";
    }

    @Nullable
    @Override
    public String getBlockCommentPrefix() {
        return "";
    }

    @Nullable
    @Override
    public String getBlockCommentSuffix() {
        return null;
    }

    @Nullable
    @Override
    public String getCommentedBlockCommentPrefix() {
        return null;
    }

    @Nullable
    @Override
    public String getCommentedBlockCommentSuffix() {
        return null;
    }
}
```

### 17.2. Register the commenter

```xml
<lang.commenter language="Simple" implementationClass="com.simpleplugin.SimpleCommenter"/>
```

### 17.3. Run the project

![Commenter](img/commenter.png)

[Previous](code_style_settings.html)
[Top](../custom_language_support_tutorial.html)
[Next](quick_fix.html)

