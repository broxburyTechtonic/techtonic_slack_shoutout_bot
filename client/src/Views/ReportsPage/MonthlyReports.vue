<template>
  <div class="monthly-reports">
    <div class="item-container">
      <v-date-picker
        class="item-container__picker"
        type="month"
        v-model="picker"
      >
      </v-date-picker>
      <DataTable
        class="item-container__table"
        :data="rankedUsers"
        :loading="loading"
        :title="tableTitle"
        :toggleable="true"
        @toggleView="toggleView"
      />
    </div>
    <RadarChart :data="rankedUsers.slice(0, 7)" class="item-container__chart" />
  </div>
</template>

<script>
import RadarChart from './Charts/RadarChart';
import DataTable from '@/components/common/DataTable';

export default {
  name: 'MonthlyReports',
  components: { DataTable, RadarChart },
  data() {
    return {
      picker: new Date().toISOString().substr(0, 7),
      shoutsToDisplay: [],
      tableView: true,
    };
  },
  created() {
    this.getRankedList();
  },
  methods: {
    createDateObj() {
      return {
        selectedMonth: this.picker.split('-')[1],
        selectedYear: this.picker.split('-')[0],
      };
    },
    getRankedList() {
      const { selectedMonth, selectedYear } = this.createDateObj();
      this.$store.dispatch('getRankedUsersByMonth', {
        type: this.tableView ? 'shouter' : 'shoutee',
        month: selectedMonth,
        year: selectedYear,
      });
    },
    selectedDate() {
      const selectedDate = new Date(
        this.picker.split('-')[0],
        this.picker.split('-')[1] - 1,
        1
      );

      return {
        month: selectedDate.toLocaleString('default', { month: 'long' }),
        year: selectedDate.toLocaleString('default', { year: 'numeric' }),
      };
    },
    toggleView() {
      this.tableView = !this.tableView;
    },
  },
  computed: {
    tableTitle() {
      const { month, year } = this.selectedDate();
      if (this.tableView)
        return `Most Shoutouts Given In ${month},
            ${year}`;
      else
        return `Most Shoutouts Received In ${month},
            ${year}`;
    },
    rankedUsers() {
      return this.$store.state.rankedUsers;
    },
    loading() {
      return this.$store.state.loading.rankedUsers;
    },
  },
  watch: {
    picker() {
      this.getRankedList();
    },
    tableView() {
      this.getRankedList();
    },
  },
};
</script>

<style lang="scss">
@use "@/assets/styles/variables.scss" as v;

.monthly-reports .apexcharts-toolbar {
  display: none;
}

.item-container {
  background-color: v.$main-bkgrnd;
  display: flex;
  justify-content: center;

  .v-data-table__wrapper {
    height: 288px;
    overflow-y: scroll !important;

    &::-webkit-scrollbar {
      width: 11px;
    }
    &::-webkit-scrollbar-track {
      background: v.$main-white;
    }
    &::-webkit-scrollbar-thumb {
      background-color: v.$main-grey;
      border-radius: 6px;
      border: 3px solid v.$main-white;
    }
    tr .text-start {
      padding: 0 0 0 50px;
    }
  }
  .v-toolbar {
    border-radius: 5px;
  }
  .table-container__title {
    height: 64px !important;
  }
  .v-picker__title {
    padding: 21px;
  }
  .v-date-picker-header {
    padding: 17.5px 16px;
  }
  &__picker {
    color: v.$accent-blue;
    height: 380px;
    margin: 1rem 5px 1rem 20px;
  }
  &__table {
    margin: 1rem 20px 1rem 5px;
    max-width: 700px;
    width: calc(100% - 320px);
  }
  &__chart {
    background-color: v.$main-bkgrnd;
    max-height: 300px;
  }
}
</style>
