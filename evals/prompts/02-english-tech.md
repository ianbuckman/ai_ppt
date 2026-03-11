# Test 02: 英文技术文章 → 英文排版 + Mood 分析

## 场景

短英文技术文章（内嵌），测试英文排版、slide count 公式、mood 分析（应为 Calm 或 Inspire）。

## Prompt

```
Make a presentation from this article:

# Why SQLite Is the Most Deployed Database

SQLite is everywhere. It runs on every smartphone, every web browser, and most embedded systems. With over one trillion active databases, SQLite is arguably the most widely deployed software component in history.

## The Design Philosophy

Richard Hipp designed SQLite with a radical idea: a database should be a single file. No server. No configuration. No admin. Just a file you can copy, email, or put on a USB drive.

This simplicity is not a limitation — it's the entire point. While PostgreSQL and MySQL compete on features, SQLite competes on being invisible. The best infrastructure is the kind you never think about.

## The Numbers

- 1 trillion+ active databases worldwide
- Used in every Android and iOS device
- Embedded in Firefox, Chrome, Safari, and Edge
- 711.2 million lines of test code for 151,000 lines of source code
- Test-to-code ratio: 4,708:1

## Why the Test Ratio Matters

Most software projects consider 1:1 test coverage ambitious. SQLite's 4,708:1 ratio reveals a different philosophy: the cost of a bug in infrastructure used by billions is astronomically higher than the cost of writing another test.

This is the same principle behind aviation safety. Airlines don't test "enough" — they test until every conceivable failure mode is covered. SQLite applies this standard to database software.

## The Lesson for Software Engineers

SQLite teaches us that simplicity and reliability are features, not compromises. In an industry obsessed with adding capabilities, SQLite's restraint is its greatest strength.

The next time you're tempted to add a feature, ask: "Would SQLite add this?" If the answer is no, you probably shouldn't either.
```

## 预期行为

1. 识别为英文技术文章
2. Mood 分析：应为 Calm（教育性、方法论）或 Inspire（思想领导力）
3. Slide count：约 400 words → 3-5 slides 偏少，实际应在 8-12 slides
4. 不应用中文排版规则
5. 数据点（1 trillion、4708:1 ratio）应有 stat slide

## 测试交互选择

- 风格选择：选 A
- Slide 计划确认：说"looks good"

## 重点检查

- 英文字体正确（无中文 fallback 作为主字体）
- line-height 和 letter-spacing 适合英文（不是中文的 1.8-2.0）
- "1 trillion+" 和 "4,708:1" 有 dramatic stat slide
- aviation safety 类比有合适的 slide type（analogy → two-column 或类似）
- 结尾 "Would SQLite add this?" 作为 closing callback
