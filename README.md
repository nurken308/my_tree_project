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
