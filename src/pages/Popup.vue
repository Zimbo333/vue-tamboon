<script setup>
import { reactive, ref } from "vue";
import { doc, updateDoc, addDoc, getDoc } from "firebase/firestore";
import { db } from "../firebase.js";

const memberId = ref("");
const point = ref(0);

const addPoint = async () => {
  if (Number.isInteger(parseInt(point.value))) {
    const memberRef = doc(db, "member", memberId.value);
    const memberSnap = await getDoc(memberRef);
    if (memberSnap.exists()) {
      const memberData = memberSnap.data();
      const newScore = memberData.score + parseInt(point.value);
      await updateDoc(memberRef, {
        score: newScore,
      });
      alert("แต้มนักเรียนถูกเพิ่มแล้ว");
      memberId.value = "";
      point.value = 0;
    } else {
      alert("ไม่พบข้อมูลสมาชิก");
    }
  } else {
    alert("กรุณากรอกคะแนนเป็นตัวเลข");
  }
};
</script>

<template>
  <div class="h-screen">
    <div class="flex justify-center items-center h-2/6 bg-violet-700">
      <h1 class="text-xl px-8 py-4 text-white  rounded-xl ">ให้แต้มนักเรียน</h1>
    </div>
    <div class="bg-amber-500 shadow-xl p-6 h-4/6">
      <div class="flex flex-col gap-4">
        <input class="rounded p-4 text-xl" v-model="memberId" type="text" placeholder="id นักเรียน"/>
        <input class="rounded p-4 text-xl" v-model.number="point" type="num" placeholder="คะแนน"/>
      </div>
      <div class="flex justify-center my-4">
        <button @click="addPoint" class="bg-violet-700 hover:bg-violet-700 text-white font-bold py-2 px-4 rounded">เพิ่มคะแนน</button>
      </div>
    </div>
    
  </div>
</template>

<style>
html,
body {
  width: 300px;
  height: 400px;
  padding: 0;
  margin: 0;
}


body > div {
  height: 100%;
}



</style>
