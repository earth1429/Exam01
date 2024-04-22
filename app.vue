<template>
  <div>
    <header>
    </header>

    <div>
      <div>
        <h2>ตัวอย่างตารางธรรมดาใน Nuxt 3</h2>

        <table border="1">
          <thead>
            <tr>
              <th>Status</th>
              <th>ลำดับ</th>
              <th>ชื่อเรื่อง</th>
              <th>วันที่สร้าง</th>
              <th>จัดการ</th>
            </tr>
          </thead>
          <tbody v-for="detail in information.data">
            <tr>
              <td>
                <button @click="toggleValue">{{ detail.Status }}</button>
                <!-- <label class="switch">
                  <input type="checkbox" @click="toggleValue">{{ detail.Status }}</input>
                  <span class="slider round"></span>
                </label> -->
                
              </td>
              <td>{{ detail.NewsId }}</td>
              <td>{{ detail.NameNews }}</td>
              <td>{{ detail.UpdatedDate }}</td>
              <td><img src="/image/view.png" alt="view"> <img src="/image/edit.png" alt="edit"> <img
                  src="/image/delete.png" alt="delete"></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
const isToggled = ref(false); // ค่าเริ่มต้น



// เรียกใช้งาน useFetch เพื่อรับข้อมูลข่าว
const { data: information } = useFetch('https://ba-sit.uapi.app/uapi/drt-ElectronicsDocument/ED-GetNews?EmployeeId=3');

const toggleValue = async () => {
  try {
    // ทำการส่งคำขอ POST หรือ PUT ไปยัง API เพื่อเปลี่ยนค่า
    await this.$axios.get('https://ba-sit.uapi.app/uapi/drt-ElectronicsDocument/ED-GetNews?EmployeeId=3', {
      Status: isToggled.Status ? 1 : 0, // ส่งค่าเป็น 1 หรือ 0 ตามค่าของ isToggled
    });

    // เมื่อสำเร็จในการอัปเดตค่าใน API แล้ว สลับค่า isToggled
    isToggled.Status = !isToggled.Status;
  } catch (error) {
    console.error('Error updating status:', error);
  }
};

console.log(information);
</script>



<style scoped>
img {
  height: 20px;
  padding: 5px;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked+.slider {
  background-color: #2196F3;
}

input:focus+.slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked+.slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>