App.vue находятся в папке src

Подробное описание TreeComponent.vue который находится в папке components

Основная функциональность
Рендеринг дерева: Компонент принимает массив узлов дерева и отображает их в виде вложенной структуры.
Состояние сворачивания/разворачивания: Компонент позволяет пользователю сворачивать и разворачивать узлы дерева, и сохраняет это состояние между рендерами и перезагрузками страницы.
Динамическое управление данными: Компонент может принимать данные динамически и обновлять дерево в реальном времени.
Структура TreeComponent.vue
Описание компонентов и их функциональности
template:

Определяет структуру HTML для компонента.
Включает кнопку "Rerender Tree" для корневого компонента.
Использует v-for для итерации по узлам дерева и отображения их.
Обрабатывает клики для сворачивания и разворачивания узлов дерева.
script:

Импортирует необходимые функции и типы из Vue.
Определяет интерфейс TreeNode для типизации узлов дерева.
Экспортирует компонент с помощью defineComponent.
props:

elements: Массив узлов дерева, передаваемый в компонент.
isRoot: Булево значение, указывающее, является ли компонент корневым.
Функции и методы:

isExpanded: Проверяет, раскрыт ли узел.
toggle: Переключает состояние раскрытия узла.
getChildren: Возвращает дочерние узлы для данного родителя.
hasChildren: Проверяет, имеет ли узел дочерние элементы.
getBackgroundColor: Определяет цвет фона для узлов в зависимости от их глубины.
rerenderTree: Выполняет ререндеринг дерева с сохранением состояния.
saveState: Сохраняет состояние раскрытых узлов в localStorage.
loadState: Загружает состояние раскрытых узлов из localStorage.
Жизненные циклы и реактивность:

onMounted: Загружает состояние узлов при монтировании компонента.
watch: Наблюдает за изменениями в состоянии раскрытых узлов и сохраняет их.
Этот файл TreeComponent.vue является основным компонентом для отображения дерева с вложенными элементами, поддерживающим сворачивание и разворачивание узлов, а также сохранение их состояния.

Components
TreeComponent.vue
TreeComponent is the main component that renders the tree structure.

Props
elements (Array): Array of tree nodes.
isRoot (Boolean, default: true): Indicates if the component is the root component.
Methods
toggle: Toggles the expansion state of a tree node.
isExpanded: Checks if a tree node is expanded.
getChildren: Gets the child nodes of a given parent node.
hasChildren: Checks if a tree node has child nodes.
getBackgroundColor: Returns the background color of a tree node based on its depth.
rerenderTree: Rerenders the tree while preserving the state of expanded nodes.
App.vue
The root component that includes the TreeComponent.

Data
elements: An array of tree nodes fetched from an API.
Methods
fetchData: Fetches tree data from an API.
addDepth: Recursively adds depth information to tree nodes based on their parent-child relationships.
