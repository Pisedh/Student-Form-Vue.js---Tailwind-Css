<template>
  <div class="p-8">
    <h1 class="text-2xl font-bold mb-10 text-center">Student Information</h1>
    <form @submit.prevent="addStudent" v-if="!isEditing" class="space-y-4">
      <div class="grid grid-cols-3 gap-4">
        <div>
          <label for="firstname" class="block font-semibold">First Name</label>
          <input
            v-model="form.firstname"
            id="firstname"
            type="text"
            class="w-full border rounded p-2"
          />
        </div>
        <div>
          <label for="lastname" class="block font-semibold">Last Name</label>
          <input
            v-model="form.lastname"
            id="lastname"
            type="text"
            class="w-full border rounded p-2"
          />
        </div>
        <div>
          <label for="gender" class="block font-semibold">Gender</label>
          <select
            v-model="form.gender"
            id="gender"
            class="w-full border rounded p-2"
          >
            <option value="" disabled>Select Gender</option>
            <option value="Male">Male</option>
            <option value="Female">Female</option>
          </select>
        </div>
      </div>
      <div class="grid grid-cols-2 gap-4">
        <div>
          <label for="email" class="block font-semibold">Email</label>
          <input
            v-model="form.email"
            id="email"
            type="email"
            class="w-full border rounded p-2"
          />
        </div>
        <div>
          <label for="address" class="block font-semibold">Address</label>
          <textarea
            v-model="form.address"
            id="address"
            rows="3"
            class="w-full border rounded p-2"
          ></textarea>
        </div>
      </div>
      <div class="w-full text-center">
        <button
          type="submit"
          class="bg-emerald-600 text-white px-4 w-2/4 py-3 rounded-lg text-center font-semibold text-md transition duration-300 hover:scale-105 hover:bg-emerald-700 focus:ring-4 focus:ring-emerald-300 focus:outline-none"
        >
          Add Student
        </button>
      </div>
    </form>

    <form @submit.prevent="saveStudent" v-if="isEditing" class="space-y-4">
      <div class="grid grid-cols-3 gap-4">
        <div>
          <label for="editFirstname" class="block font-semibold">First Name</label>
          <input
            v-model="form.firstname"
            id="editFirstname"
            type="text"
            class="w-full border rounded p-2"
          />
        </div>
        <div>
          <label for="editLastname" class="block font-semibold">Last Name</label>
          <input
            v-model="form.lastname"
            id="editLastname"
            type="text"
            class="w-full border rounded p-2"
          />
        </div>
        <div>
          <label for="editGender" class="block font-semibold">Gender</label>
          <select
            v-model="form.gender"
            id="editGender"
            class="w-full border rounded p-2"
          >
            <option value="" disabled>Select Gender</option>
            <option value="Male">Male</option>
            <option value="Female">Female</option>
          </select>
        </div>
      </div>
      <div class="flex justify-end space-x-4">
        <button
          type="button"
          @click="cancelEdit"
          class="bg-gray-500 text-white px-4 py-2 rounded-lg font-semibold hover:bg-gray-600 focus:ring-4 focus:ring-gray-300 focus:outline-none"
        >
          Cancel
        </button>
        <button
          type="submit"
          class="bg-blue-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-blue-700 focus:ring-4 focus:ring-blue-300 focus:outline-none"
        >
          Save
        </button>
      </div>
    </form>

    <div v-if="students.length" class="mt-8">
      <h2 class="text-xl font-bold mb-4 text-center">Student List</h2>
      <table class="w-full border-collapse border border-gray-300">
        <thead>
          <tr class="bg-yellow-400">
            <th class="border border-gray-300 p-2">First Name</th>
            <th class="border border-gray-300 p-2">Last Name</th>
            <th class="border border-gray-300 p-2">Gender</th>
            <th class="border border-gray-300 p-2">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(student, index) in students" :key="index">
            <td class="border border-gray-300 p-2">{{ student.firstname }}</td>
            <td class="border border-gray-300 p-2">{{ student.lastname }}</td>
            <td class="border border-gray-300 p-2">{{ student.gender }}</td>
            <td class="border border-gray-300 p-2">
              <button
                @click="editStudent(index)"
                class="bg-green-500 text-white px-5 py-1 rounded-md hover:bg-green-600 focus:ring-4 focus:ring-green-300 focus:outline-none"
              >
                Edit
              </button>
              <button
                @click="confirmDelete(index)"
                class="bg-red-500 text-white px-2 py-1 rounded-md hover:bg-red-600 focus:ring-4 focus:ring-red-300 focus:outline-none"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        firstname: "",
        lastname: "",
        gender: "",
        email: "",
        address: "",
      },
      students: [],
      isEditing: false,
      editIndex: null,
    };
  },
  methods: {
    addStudent() {
      if (!this.form.firstname || !this.form.lastname) {
        alert("Enter student's first and last name");
        return;
      }
      this.students.push({ ...this.form });
      this.resetForm();
    },
    editStudent(index) {
      this.isEditing = true;
      this.editIndex = index;
      this.form = { ...this.students[index] };
    },
    saveStudent() {
      if (this.editIndex !== null) {
        this.students.splice(this.editIndex, 1, { ...this.form });
      }
      this.cancelEdit();
    },
    cancelEdit() {
      this.resetForm();
      this.isEditing = false;
      this.editIndex = null;
    },
    confirmDelete(index) {
      if (confirm("Are you sure you want to delete this student?")) {
        this.deleteStudent(index);
      }
    },
    deleteStudent(index) {
      this.students.splice(index, 1);
    },
    resetForm() {
      this.form = {
        firstname: "",
        lastname: "",
        gender: "",
        email: "",
        address: "",
      };
    },
  },
};
</script>
