<template>
  <div>
    <!-- Кнопка ререндеринга только в корневом компоненте -->
    <div v-if="isRoot">
      <button @click="rerenderTree">Rerender Tree</button>
    </div>
    <!-- Отображение элементов дерева -->
    <div v-for="item in elements" :key="item.id" :style="{ paddingLeft: `${item.depth * 20}px`, backgroundColor: getBackgroundColor(item) }">
      <div @click="toggle(item.id)" style="cursor: pointer;">
        <!-- Показать/скрыть символ для узлов с дочерними элементами -->
        <span v-if="hasChildren(item.id)">{{ isExpanded(item.id) ? '▼' : '▶' }}</span>
        {{ item.title }}
      </div>
      <!-- Рекурсивный рендеринг дочерних узлов -->
      <div v-if="isExpanded(item.id)">
        <TreeComponent :elements="getChildren(item.id)" :isRoot="false" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch, onMounted, nextTick } from 'vue';

interface TreeNode {
  title: string;
  parent_id: string | null;
  id: string;
  depth: number;
}

export default defineComponent({
  name: 'TreeComponent',
  props: {
    elements: {
      type: Array as () => TreeNode[],
      required: true
    },
    isRoot: {
      type: Boolean,
      default: true
    }
  },
  setup(props) {
    const expandedNodes = ref<string[]>([]); // Список раскрытых узлов

    // Проверка, раскрыт ли узел
    const isExpanded = (id: string) => expandedNodes.value.includes(id);
    
    // Переключение состояния раскрытия узла
    const toggle = (id: string) => {
      if (isExpanded(id)) {
        expandedNodes.value = expandedNodes.value.filter(nodeId => nodeId !== id);
      } else {
        expandedNodes.value.push(id);
      }
    };

    // Получение дочерних узлов для данного родителя
    const getChildren = (parentId: string) => {
      return props.elements.filter(element => element.parent_id === parentId).map(child => ({
        ...child,
        depth: props.elements.find(element => element.id === parentId)?.depth! + 1
      }));
    };

    // Проверка, имеет ли узел дочерние элементы
    const hasChildren = (id: string) => {
      return props.elements.some(element => element.parent_id === id);
    };

    // Определение цвета фона для узлов
    const getBackgroundColor = (item: TreeNode) => {
      return item.depth % 2 === 0 ? '#f0f0f0' : '#ffffff';
    };

    // Функция ререндеринга дерева с сохранением состояния
    const rerenderTree = () => {
      const currentExpandedNodes = [...expandedNodes.value];
      nextTick(() => {
        expandedNodes.value = currentExpandedNodes;
      });
    };

    // Сохранение состояния раскрытых узлов в localStorage
    const saveState = () => {
      localStorage.setItem('expandedNodes', JSON.stringify(expandedNodes.value));
    };

    // Загрузка состояния раскрытых узлов из localStorage
    const loadState = () => {
      const savedState = localStorage.getItem('expandedNodes');
      if (savedState) {
        expandedNodes.value = JSON.parse(savedState);
      }
    };

    // Загрузка состояния при монтировании компонента
    onMounted(() => {
      loadState();
    });

    // Сохранение состояния при изменении списка раскрытых узлов
    watch(expandedNodes, saveState);

    return {
      expandedNodes,
      toggle,
      isExpanded,
      getChildren,
      hasChildren,
      getBackgroundColor,
      rerenderTree
    };
  }
});
</script>

<style scoped>
/* Добавьте ваши стили здесь */
</style>
