<template>
	<div :class="{ isHide: isHide }" class="m-pop-up pop-up-detail">
		<div class="pop-up-modal"></div>
		<div class="pop-up-content">
			<div
				style="margin: 0px 40px;
    width: calc(100% - 80px);
    border-bottom: 1px solid #bbb;"
			>
				<div class="icon-danger"></div>
				<div class="pop-up-body">
					<div>{{ content }}</div>
				</div>
			</div>
			<div class="pop-up-header">
				<div class="pop-up-header-title">{{ header }}</div>
				<div class="pop-up-header-close">
					<button title="Esc" v-on:click="btnCancelOnClick">
						<b>x</b>
					</button>
				</div>
			</div>
		</div>
		<div v-if="this.type === 'danger'" class="pop-up-footer">
			<button
				title="Esc"
				style="background-color: #fff; border: 1px solid #bbb; color: #111; font-weight: bold"
				v-on:click="btnCancelOnClick"
				id="btnCancel"
				class="m-btn m-btn-default m-btn-cancel "
			>
				Không
			</button>
			<button
				title="Enter"
				@keyup.enter="btnDeleteOnClick(className, data)"
				@click="btnDeleteOnClick(className, data)"
				id="btnSave"
				class="m-btn m-btn-default "
			>
				<span class="">Có</span>
			</button>
		</div>
		<div
			title="Esc"
			v-else-if="this.type === 'notify'"
			class="pop-up-footer"
		>
			<button @click="btnCancelOnClick" id="btnSave" class="m-btn ">
				<span class="">Đóng</span>
			</button>
		</div>
		<div v-else class="pop-up-footer">
			<button
				@click="btnCancelOnClick"
				id="btnSave"
				class="m-btn m-btn-default m-btn-cancel"
			>
				<span class="">Tiếp tục nhập</span>
			</button>
			<button
				@keyup.enter="btnAddOnClick"
				v-on:click="btnAddOnClick"
				id="btnCancel"
				class="m-btn m-btn-default "
			>
				Đóng
			</button>
		</div>
	</div>
</template>

<script>
export default {
	created() {
		// Hàm xử lý sự kiện khi người dùng nhấn để đóng pop up
		window.addEventListener("keyup", this.changeActiveTabDailog);
	},
	props: ["isHide", "header", "type", "content", "className", "data"],

	data() {
		return {};
	},

	computed: {},

	components: {},
	methods: {
		// Hàm xử lý sự kiện người dùng đồng ý xóa
		btnDeleteOnClick(className, id) {
			this.$emit("notifyDanger", {
				accept: true,
				className: className,
				id: id,
			});
		},

		// Hàm xử lý sự kiện người dùng nhấn Đóng của thông báo
		changeActiveTabDailog(event) {
			if (this.type == "notify") {
				if (event.which === 13 || event.which == 27)
					this.btnCancelOnClick();
			} else if (this.type == "danger" && this.isHide == false) {
				if (event.which === 27) this.btnCancelOnClick();
				else if (event.which == 13) {
					this.btnDeleteOnClick(this.className, this.data);
				}
			}
		},

		// * Hàm xử lý sự kiện khi nhấn nút click
		async btnAddOnClick() {
			this.$emit("notify", true);
		},

		// * Hàm xử lý sự kiện khi bấm nút hủy
		btnCancelOnClick() {
			this.$emit("notifyDanger", {
				accept: false,
			});
		},
	},
};
</script>

<style scope>
.isHide {
	display: none;
}
.m-pop-up {
	z-index: 1000;
	position: fixed;
	top: 0;
	right: calc(50% - 200px);
	width: 400px;
	height: 137px;
	top: calc(50% - 137px);
}

.pop-up-modal {
	position: fixed;
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	background-color: black;
	opacity: 0.4;
	z-index: -1;
}

.m-pop-up .pop-up-content {
	background-color: #ffffff;
	z-index: 1000;
	border-radius: 5px 5px 0 0px;

	width: 100%;
	height: calc(100% - 15px);
	position: relative;
	display: flex;
	margin: 0 auto;
}
.m-pop-up .pop-up-content + div {
	width: calc(100% - 24px);
	height: calc(100% - 25px);
	padding: 24px;
}

.m-pop-up .pop-up-content .icon-danger {
	background: url("../../assets/content/icon/Sprites.64af8f61.svg") no-repeat -598px -463px;
	width: 70px;
	height: 50px;
	margin-right: 20px;
	margin-left: 20px;
	position: absolute;
	top: 40px;
	left: 20px;
}

.m-pop-up .pop-up-content .pop-up-body {
	width: 352px;
	height: 63px;
	display: flex;
}

.m-pop-up .pop-up-content .pop-up-body div {
	position: absolute;
	top: 37px;
	height: 85px;
	width: 262px;
	left: 95px;
}

.m-pop-up .pop-up-header {
	display: flex;
}

.m-pop-up .pop-up-header .pop-up-header-close {
	position: absolute;
	right: 0;
	top: 0;
}

.danger {
	background-color: red !important;
}

.m-pop-up .pop-up-header .pop-up-header-close:hover {
	border-radius: 5px;
}

.m-pop-up .pop-up-header .pop-up-header-close button {
	width: 40px;
	height: 40px;
	border-radius: 0 5px 0 0;
	border: none;
	outline: none;
	cursor: pointer;
	background-color: #ffffff;
	background-image: url("../../assets/content/icon/x.svg");
	background-repeat: no-repeat;
	background-position: center;
	background-size: 20px;
	opacity: 0.7;
}

.m-pop-up .pop-up-header .pop-up-header-close button:hover {
	background-color: #ccc;
}

/*Tiêu đề của pop-up*/

.m-pop-up .pop-up-header .pop-up-header-title {
	font-size: 15px;
	font-weight: bold;
}

.pop-up-footer {
	display: flex;
	width: 100% !important;
	height: 89px !important;
	border-radius: 0 0 5px 5px;
	align-items: center;
	justify-content: flex-end;
	padding: 24px 24px;
	background-color: #ffffff;
	box-sizing: border-box;
	border: none !important;
}

.pop-up-footer button {
	margin-left: 16px;
}
.pop-up-footer button span {
	text-align: center;
}
</style>
