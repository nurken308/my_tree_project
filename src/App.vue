<template>
  <div id="app">
    <TreeComponent :elements="elementsWithDepth" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios';
import TreeComponent from './components/TreeComponent.vue';

interface TreeNode {
  title: string;
  parent_id: string | null;
  id: string;
  depth: number;
}

export default defineComponent({
  name: 'App',
  components: {
    TreeComponent
  },
  setup() {
    const elements = ref<TreeNode[]>([]);

    // Функция для получения данных с сервера
    const fetchData = async () => {
      try {
        const response = await axios.get('https://64b4c8450efb99d862694609.mockapi.io/tree/items');
        elements.value = response.data;
        addDepth(elements.value);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    // Добавление уровня вложенности для каждого элемента
    const addDepth = (elements: TreeNode[]) => {
      const addDepthRecursively = (nodes: TreeNode[], parentId: string | null, depth: number) => {
        nodes.filter(node => node.parent_id === parentId).forEach(node => {
          node.depth = depth;
          addDepthRecursively(nodes, node.id, depth + 1);
        });
      };
      addDepthRecursively(elements, null, 0);
    };

    // Получение данных при монтировании компонента
    onMounted(() => {
      fetchData();
    });

    return {
      elementsWithDepth: elements
    };
  }
});
</script>

<style scoped>
/* Добавьте ваши стили здесь */
</style>
