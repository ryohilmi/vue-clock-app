<script>
export default {
    props: {
        alarmId: Number,
        alarm: {
            type: Object
        }
    },
    methods: {
        log(e) {
            console.log(e.target.checked);
        }
    }
}
</script>

<template>
    <div class="alarm-item">
        <span>{{ alarm.hour }}:{{ alarm.minute.toString().padStart(2, '0') }}</span>
        <input type="checkbox" name="" :id="alarmId" @change="log($event)" :checked="alarm.isActive">
        <label :for="alarmId"></label>
    </div>
</template>

<style scoped>
.alarm-item {
    background: white;
    width: 90%;
}

input {
	height: 0;
	width: 0;
	visibility: hidden;
}

label {
	cursor: pointer;
	text-indent: -9999px;
	width: 200px;
	height: 100px;
	background: grey;
	display: block;
	border-radius: 100px;
	position: relative;
}

label:after {
	content: '';
	position: absolute;
	top: 5px;
	left: 5px;
	width: 90px;
	height: 90px;
	background: #fff;
	border-radius: 90px;
	transition: 0.3s;
}

input:checked + label {
	background: #bada55;
}

input:checked + label:after {
	left: calc(100% - 5px);
	transform: translateX(-100%);
}

label:active:after {
	width: 130px;
}
</style>