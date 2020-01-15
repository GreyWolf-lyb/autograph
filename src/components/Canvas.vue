<template>
	<div id="canvasBox" :style="getHorizontalStyle">
		<div class="image-box" v-show="showBox">
			<header>
				<input type="button" value="返回" @click="showBox = false" />
			</header>
			<img :src="signImage">
		</div>
		<div class="greet">
			<span>{{msg}}</span>
			<a @touchstart="clear" @mousedown="clear">清屏</a>
			<span></span>
			<a @touchstart="download" @mousedown="download">下载</a>
			
			<input type="button" value="确认" @touchstart="savePNG" @mousedown="savePNG"/>
		</div>
		<canvas></canvas>
	</div>
</template>

<script>
	import Draw from '../utils/draw';

	export default {
		name: 'canvas',
		data() {
			return {
				msg: '请签字：',
				degree: 0,
				showBox: false,
				signImage: null,
			};
		},
		components: {
			Draw,
		},
		mounted() {
			this.canvasBox = document.getElementById('canvasBox');
			this.initCanvas();
		},
		computed: {
			getHorizontalStyle() {
				const d = document;
				const w = window.innerWidth || d.documentElement.clientWidth || d.body.clientWidth;
				const h = window.innerHeight || d.documentElement.clientHeight || d.body.clientHeight;
				let length = (h - w) / 2;
				let width = w;
				let height = h;

				switch (this.degree) {
					case -90:
						length = -length;
					case 90:
						width = h;
						height = w;
						break;
					default:
						length = 0;
				}
				if (this.canvasBox) {
					this.canvasBox.removeChild(document.querySelector('canvas'));
					this.canvasBox.appendChild(document.createElement('canvas'));
					setTimeout(() => {
						this.initCanvas();
					}, 200);
				}
				return {
					transform: `rotate(${this.degree}deg) translate(${length}px,${length}px)`,
					width: `${width}px`,
					height: `${height}px`,
					transformOrigin: 'center center',
				};
			},
		},
		methods: {
			savePNG() {
				this.signImage = this.draw.getPNGImage();
				this.showBox = true;
			},
			initCanvas() {
				const canvas = document.querySelector('canvas');
				this.draw = new Draw(canvas, -this.degree);
			},
			clear() {
				this.draw.clear();
			},
			download() {
				this.draw.downloadPNGImage(this.draw.getPNGImage());
			},
			upload() {
				const image = this.draw.getPNGImage();
				const blob = this.draw.dataURLtoBlob(image);

				const url = '';
				const successCallback = (response) => {
					console.log(response);
				};
				const failureCallback = (error) => {
					console.log(error);
				};
				this.draw.upload(blob, url, successCallback, failureCallback);
			},
		},
	};
</script>

<style>
	#canvasBox {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.greet {
		padding: 20px;
		font-size: 20px;
		user-select: none;
	}

	.greet a {
		cursor: pointer;
	}

	.greet select {
		font-size: 18px;
	}

	canvas {
		flex: 1;
		cursor: crosshair;
		border: 2px dashed lightgray;
	}
</style>
