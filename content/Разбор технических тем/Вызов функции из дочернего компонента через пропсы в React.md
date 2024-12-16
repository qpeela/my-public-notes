
- Иногда возникает необходимость рендерить контент в дочернем компоненте в зависимости от того, где используется этот компонент. Например, у нас есть карточки для продуктов: фрукты и овощи. Содержимое этих карточек немного отличается, и в зависимости от состояния карточки (например, свежесть или целостность продукта) изменяется верстка и логика обработки событий

#### Рассмотрим пример

1. Компонент для фруктов (FruitCardComponent)
Компонент **FruitCardComponent** рендерит карточку фрукта. В зависимости от состояния фрукта (свежий или испорченный), выполняется соответствующее действие.

```typescript

import React from 'react';
import Card from './Card';

const STATUSES = {
    fresh: 1,
    bad: 2,
};

const FruitCardComponent = () => {
    const handleChildAction = ({ statusFruit }) => {
        if (statusFruit === STATUSES.fresh) {
            callSomeApi(); // Логика для свежего фрукта
        } else {
            destroyFruit(); // Логика для испорченного фрукта
        }
    };

    return (
        <div>
            <h1>Fruit Component</h1>
            <Card onAction={handleChildAction} />
        </div>
    );
};

export default FruitCardComponent;

```

2. Компонент для овощей (VegetableCardComponent)
Компонент **VegetableCardComponent** рендерит карточку овоща. Если овощ целый, его можно нарезать.

```typescript

import React from 'react';
import Card from './Card';

const STATUSES = {
    whole: 1,
    cutted: 2,
};

const VegetableCardComponent = () => {
    const handleChildAction = ({ status }) => {
        if (status === STATUSES.whole) {
            cutVegetable(); // Логика для целого овоща
        }
    };

    return (
        <div>
            <h1>Vegetable Component</h1>
            <Card onAction={handleChildAction} />
        </div>
    );
};

export default VegetableCardComponent;

```

***
3. Универсальный компонент Card
Компонент **Card** используется как для фруктов, так и для овощей. Он отображает имя и количество, а также передает событие `onAction` при клике на кнопку.

```typescript

import React from 'react';

const Card = ({ name, count, onAction, status }) => {
    return (
        <div>
            <h1>{name}</h1>
            <p>Кол-во: {count}</p>
            <button onClick={onAction.bind(this, { status })}>Нажать</button>
        </div>
    );
};

export default Card;

```

#### Как это работает?

1. **Универсальность компонента Card**: Компонент **Card** не зависит от типа продукта (фрукты или овощи). Все действия происходят на уровне родительского компонента, который определяет, что делать с событием `onAction`.

2. **Гибкость в обработке событий**: Карточка передает событие `onAction` с данными `status` обратно в родительский компонент, а тот решает, какую логику применить.


---
#### Итог

Теперь компонент **Card** можно использовать для рендеринга карточек любых продуктов. Его задача — отобразить базовую структуру (имя, количество и кнопку). Логика же полностью делегируется родительскому компоненту через `props`. Такой подход делает компонент более универсальным и повторно используемым.
