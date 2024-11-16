---
created: 2024-10-01T20:25
updated: 2024-11-16T09:07
aliases:
  - article
  - Статья
  - Typescript
  - разбор технических тем по работе
Автор: https://weser.io/blog/clean-react-with-typescript?utm_source=newsletter.reactdigest.net&utm_medium=newsletter&utm_campaign=clean-react-with-typescript&_bhlid=adb5fa0021a338da6b36ce8527e5e2b4cea12bb2
---
## Article React-Typescript Tips

### `PropsWithChildren`
Если мы использовать children у React, чтобы не прописывать тип `children: ReactNode`, можно использовать дженерик `PropsWithChildren<>`

```typescript
import { ReactNode, PropsWithChildren } from 'react';

interface Props {
   title: string;
}

const Title = ({title, children}: PropsWithChildren<Props>) => {
	return (
		<div>
			<div>{title}</div>  
			{children}
		</div>
	)
}

```

#### Получить тип всех `props` у компонента можно через дженерик `ComponentProps<>`, которому можно передать тип нужного нам компонента `typeof ComponentName`

```typescript
import { ComponentProps } from 'react';

import ProductTile from './ProductTile';

type Props = {
  color: string
} & ComponentProps<typeof ProductTile>;

function ProminentProductTile({ color, ...props }: Props) {
  return (
    <div style={{ background: color }}>
      <ProductTile {...props} />
    </div>
  )
}
```

##### Чтобы соблюдать DRY при объявление повторяющих типов, можно выбирать тип props у компонента. В примере ниже, мы берем тип `props title` у компонента `ProductTile`

```typescript
import { ComponentProps } from 'react'

import ProductTile from './ProductTile'

type ProductTileProps = ComponentProps<typeof ProductTile>
type Props = {
  color: string
  title: ProductTileProps['title']
}
```

Также можно выбрать несколько типов, можно использовать `Pick<>`

```typescript
type ProductTileProps = ComponentProps<typeof ProductTile>;

type Props = {
  color: string
} & Pick<ProductTileProps, 'title' | 'description'>;
```

4. Можно также через дженерик `ComponentProps<>` получить тип для пропсов для html элементов, например `ComponentProps<'button'>`

5. Для передачи JSX верстки как `props`, можно использовать тип `ReactNode`

```typescript
import { PropsWithChildren, ReactNode } from 'react'

type Props = {
  title: string
  sidebar: ReactNode
}

function Layout({ title, children, sidebar }: PropsWithChildren<Props>) {
  return (
    <>
      <div className="sidebar">{sidebar}</div>
      <main className="content">
        <h1>{title}</h1>
        {children}
      </main>
    </>
  )
}
```

```typescript
const sidebar = (
  <div>
    <a data-selected href="/shoes">
      Shoes
    </a>
    <a href="/watches">Watches</a>
    <a href="/shirts">Shirts</a>
  </div>
)

const App = (
  <Layout title="Running shoes" sidebar={sidebar}>
    {/* Page content */}
  </Layout>
)
```

6. Еще есть типы для обработки нажатия (`import { MouseEventHandler } from 'react'`)
	1. `button` - `MouseEventHandler<HTMLButtonElement>`
	2. form onSubmit - `React.FormEvent<HTMLFormElement>`

---

> [!tip] Links
> Source:
> Next:

### BIO
