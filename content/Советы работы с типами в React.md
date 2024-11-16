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

### Удобный тип `PropsWithChildren` для работы с `children`

Если вы используете `children` в React, чтобы не прописывать тип `children: ReactNode`, можно воспользоваться дженериком `PropsWithChildren<>`.

```typescript
import { ReactNode, PropsWithChildren } from 'react';

interface Props {
   title: string;
}

const Title = ({ title, children }: PropsWithChildren<Props>) => {
  return (
    <div>
      <div>{title}</div>  
      {children}
    </div>
  );
};
```
### Получение типов всех `props` компонента через `ComponentProps<>`

Используйте дженерик `ComponentProps<>`, чтобы передать тип нужного компонента через `typeof ComponentName`.

```typescript
import { ComponentProps } from 'react';

import ProductTile from './ProductTile';

type Props = {
  color: string;
} & ComponentProps<typeof ProductTile>;

function ProminentProductTile({ color, ...props }: Props) {
  return (
    <div style={{ background: color }}>
      <ProductTile {...props} />
    </div>
  );
}
```

### Повторное использование типов с помощью выбора `props`

Чтобы соблюдать принцип DRY при объявлении повторяющихся типов, можно выбрать тип `props` у другого компонента.

В данном примере берется тип `title` из `props` компонента `ProductTile`.

```typescript
import { ComponentProps } from 'react';

import ProductTile from './ProductTile';

type ProductTileProps = ComponentProps<typeof ProductTile>;
type Props = {
  color: string;
  title: ProductTileProps['title'];
};
```

Выбор нескольких свойств с помощью `Pick<>`

```typescript
type ProductTileProps = ComponentProps<typeof ProductTile>;

type Props = {
  color: string;
} & Pick<ProductTileProps, 'title' | 'description'>;
```

### Типы для HTML элементов

Через `ComponentProps<>` можно получить типы для стандартных HTML элементов. Например:

```typescript
type ButtonProps = ComponentProps<'button'>;
```

### Тип для передачи JSX как `props`

Для передачи JSX как `props` используется тип `ReactNode`.

```typescript
import { PropsWithChildren, ReactNode } from 'react';

type Props = {
  title: string;
  sidebar: ReactNode;
};

function Layout({ title, children, sidebar }: PropsWithChildren<Props>) {
  return (
    <>
      <div className="sidebar">{sidebar}</div>
      <main className="content">
        <h1>{title}</h1>
        {children}
      </main>
    </>
  );
}
```

 Пример использования

```typescript
const sidebar = (
  <div>
    <a data-selected href="/shoes">Shoes</a>
    <a href="/watches">Watches</a>
    <a href="/shirts">Shirts</a>
  </div>
);

const App = (
  <Layout title="Running shoes" sidebar={sidebar}>
    {/* Page content */}
  </Layout>
);
```

##### Типы для обработки событий

Используйте специализированные типы для обработки событий, например:

1. Для кнопки: `MouseEventHandler<HTMLButtonElement>`.
2. Для формы `onSubmit`: `React.FormEvent<HTMLFormElement>`.

---

> [!tip] **Links**
> - **Source**:  
> - **Next**:  

## BIO