<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-interests')"
      :mode="storedInterestsButtonMode"
      >Stored Interests</base-button
    >
    <base-button
      @click="setSelectedTab('add-interest')"
      :mode="addInterestButtonMode"
      >Add New Interests</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredInterests from './StoredInterests.vue';
import AddInterest from './AddInterest.vue';

export default {
  components: {
    'stored-interests': StoredInterests,
    'add-interest': AddInterest,
  },
  data() {
    return {
      selectedTab: 'stored-interests',
      storedInterests: [
        {
          id: 'favourite-album',
          title: 'MadVillainy - Madvillain',
          description: 'This is one of my favourite albums.',
          link: 'https://en.wikipedia.org/wiki/Madvillainy',
        },
        {
          id: 'favourite-past-time',
          title: 'Fishing',
          description:
            'One of the greatest hobbies of all time. I love catching bass.',
          link: 'https://media.tenor.com/eXn-LAXYF5YAAAAS/pog-pogfish.gif',
        },
      ],
    };
  },
  provide() {
    return {
      interests: this.storedInterests,
      addInterest: this.addNewInterest,
      removeInterest: this.removeInterest,
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addNewInterest(newTitle, newDesc, newLink) {
      const newInterest = {
        id: new Date().toISOString(),
        title: newTitle,
        description: newDesc,
        link: newLink,
      };
      this.storedInterests.unshift(newInterest);
      this.selectedTab = 'stored-interests';
    },
    removeInterest(interestId) {
      // This does not work as it overrides storedInterests
      // with a brand new array which is not provided to the
      // rest of the components.
      // this.storedInterests = this.storedInterests.filter(
      //   (interest) => interest.id !== interestId
      // );
      // Alternative approach:
      const intIndex = this.storedInterests.findIndex(
        (int) => int.id === interestId
      );
      this.storedInterests.splice(intIndex, 1);
    },
  },
  computed: {
    storedInterestsButtonMode() {
      return this.selectedTab === 'stored-interests' ? null : 'flat';
    },
    addInterestButtonMode() {
      return this.selectedTab === 'add-interest' ? null : 'flat';
    },
  },
};
</script>
