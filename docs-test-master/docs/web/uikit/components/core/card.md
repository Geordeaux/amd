---
id: card
title: Card
---

## Overview

Universal card container

## Best practices

The most basic card container, which can hold text, lists, pictures, and paragraphs, and is often used in the background overview page.

## Usage

```tsx live title=Basic enableSwitchTheme enableExportToCodePen enableHideEditor
<Card width={256}>
  <Image src='https://images.unsplash.com/photo-1462331940025-496dfbfc7564?w=2048&q=20' />
  <Text>Card</Text>
</Card>
```

```tsx live title=Grid,Layout enableSwitchTheme enableExportToCodePen enableHideEditor
<GridRow gutter={10}>
  <GridCol span={6}>
    <Card >
      <Image src='https://images.unsplash.com/photo-1462331940025-496dfbfc7564?w=2048&q=20' />
      <Text>Card 1</Text>
    </Card>
  </GridCol>
  <GridCol span={6}>
    <Card>
      <Image src='https://images.unsplash.com/photo-1462331940025-496dfbfc7564?w=2048&q=20' />
      <Text>Card 2</Text>
    </Card>
  </GridCol>
  <GridCol span={6}>
    <Card>
      <Image src='https://images.unsplash.com/photo-1462331940025-496dfbfc7564?w=2048&q=20' />
      <Text>Card 3</Text>
    </Card>
  </GridCol>
  <GridCol span={6}>
    <Card>
      <Image src='https://images.unsplash.com/photo-1462331940025-496dfbfc7564?w=2048&q=20' />
      <Text>Card 4</Text>
    </Card>
  </GridCol>
</GridRow>
```

## Implementation

> See: [Box](/docs/web/uikit/components/box), for more Apis.
