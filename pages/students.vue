<template>
  <div>
    <h1>Список студентів</h1>
    <!-- Пошук -->
    <input type="text" v-model="searchQuery" placeholder="Пошук студента" style="margin-bottom: 10px; color: white;">

    <!-- Таблиця зі списком студентів -->
    <table>
      <thead>
      <tr>
        <th>Ім'я</th>
        <th>Прізвище</th>
        <th>Група</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="student in filteredStudents" :key="student.id">
        <td>{{ student.firstName }}</td>
        <td>{{ student.lastName }}</td>
        <td>{{ student.group }}</td>
      </tr>
      </tbody>
    </table>

    <!-- Пагінація -->
    <button @click="previousPage" :disabled="currentPage === 1">Попередня сторінка</button>
    <span>Сторінка {{ currentPage }} з {{ totalPages }}</span>
    <button @click="nextPage" :disabled="currentPage === totalPages">Наступна сторінка</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      students: [], // Список студентів
      searchQuery: '', // Пошуковий запит
      currentPage: 1, // Поточна сторінка
      itemsPerPage: 10 // Кількість елементів на сторінці
    };
  },
  computed: {
    // Обчислення загальної кількості сторінок
    totalPages() {
      return Math.ceil(this.filteredStudents.length / this.itemsPerPage);
    },
    // Фільтрація студентів за пошуковим запитом
    filteredStudents() {
      return this.students.filter(student =>
        student.firstName.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
        student.lastName.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
        student.group.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    // Відфільтрований список студентів для поточної сторінки
    paginatedStudents() {
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return this.filteredStudents.slice(startIndex, endIndex);
    }
  },
  methods: {
    // Перехід на попередню сторінку
    previousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    // Перехід на наступну сторінку
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    }
  },
  mounted() {
    // Метод для отримання списку студентів (може бути AJAX-запит)
    // Приклад:
    this.students = [
      { id: 1, firstName: 'Іван', lastName: 'Петров', group: 'Група 1' },
      { id: 2, firstName: 'Марія', lastName: 'Іванова', group: 'Група 2' },
      // Додайте інші дані за потреби
    ];
  }
};
</script>

<style>
table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  color: black;
  background-color: #f2f2f2;
}

/* Зміна кольору рядків при наведенні миші */
tr:hover {
  color: black;
  background-color: lightgray;
}
</style>
