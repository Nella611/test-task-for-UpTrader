# test-task-for-UpTrader

### [Задача](https://docs.google.com/document/d/1at2SFBLVmATyMCCYE0cSsDTTKR3na3ugTvUD5oe-ri8/edit?usp=sharing) -  сделать django app, который будет реализовывать древовидное меню

### Решение:

1. Создаём две модели:

   отдельно Menu  - по его имени у нас программа отрисовывает пункты
   
   и

   MenuItem - это пункты меню, в нем мы храним основную информацию
2. Для редактирования в в стандартной админке Django регистрируем обе модели в [admin.py](menus%2Fadmin.py)
3. дальше мы создаем template tag draw_menu:

   * для того, чтобы запрос к БД создавался один раз, определяем текущее меню через prefetch_related
   * определяем самые верхние пункты (это случай, когда ни одно меню не выбрано, но первый уровень отображается)
   * определяем все item для текущего меню (без верхних пунктов)
   * провряем, есть ли активный пункт меню (чтобы программа в дальнейшем не зациклилась, если у нас не выбран ни один пункт)
4. Логика тега построена через [menu.html](menus%2Ftemplates%2Fmenu.html), в котором:

   * eсли есть активный пункт, то отрисовываем [menu_item.html](menus%2Ftemplates%2Fmenu_item.html)
   * активный пункт определяется исходя из URL текущей страницы и url пункта
   * если нет, то рисуем только верхние пункты (у которых нет родителей, они корневые для меню)

5. В [menu_item.html](menus%2Ftemplates%2Fmenu_item.html):

   * если это выбранный пункт, то рисуем его плюс его детей (т.е. первый пункт вложенностей)
   * если нет, то рекурсивно отрисовываем пункты меню

Таким образом на любой странице можно обратиться к меню ( или всем меню, меню, или к определенным) и через  {% draw_menu menu.menu_name %} построить уже готовый список 

## Как собрать приложение

`
poetry build 
`
## Запускаем django сервер

`
 poetry run python manage.py runserver 
`
