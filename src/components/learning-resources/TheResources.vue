<template>
  <base-card>
    <base-button
      :mode="storedResButtonMode"
      @click="setSelectedTab('stored-resources')"
      >Stored Resource</base-button
    >
    <base-button
      :mode="addResButtonMode"
      @click="setSelectedTab('add-resource')"
      >Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: { StoredResources, AddResource },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: JSON.parse(localStorage.getItem('resources')) || [
        {
          id: 'official guide',
          title: 'Official Guide',
          description:
            'Switch tab and add a new resource it will persit or click the resource link to check out the repo for this project',
          link: 'https://github.com/SantosBright/resources-app-vuejs'
        }
      ]
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    }
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, link) {
      const newResource = {
        id: new Date().toISOString(),
        title,
        description,
        link
      };

      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
      localStorage.setItem('resources', JSON.stringify(this.storedResources));
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(res => res.id === resId);
      this.storedResources.splice(resIndex, 1);
      localStorage.setItem('resources', JSON.stringify(this.storedResources));
    }
  }
};
</script>

<style></style>
