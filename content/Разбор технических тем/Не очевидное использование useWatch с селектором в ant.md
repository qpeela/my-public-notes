

Когда нам нужно вычислить какое-то значение на основе значений из формы можно и нужно использовать селектор для этого. 

Например у нас было так:

```js
const carriages = Form.useWatch('carriages', form);
const selectedCarriages = carriages.filter(record => record.selected);
const countSelectedCarriages = selectedCarriages.length;
```
 
можно изменить на такое решение, которое будет [[Оптимизация в React приложений|мемоинизированное]]

```js
const countSelectedCarriages = Form.useWatch(({ carriages }) => {
  const selectedCarriages = carriages.filter(record => record.selected);
  
  return selectedCarriages.length;
}, form);
```  


***


> [!NOTE] Онтология
> ID: 202411161015
> Source::
> Next::


**Список источников:**