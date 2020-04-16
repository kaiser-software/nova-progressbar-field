<template>
    <span ref="chartContainer"></span>
</template>

<script>
	var ProgressBar = require("progressbar.js");

	export default {
		props: ["resourceName", "field"],
		data() {
			return {
				options: {
					strokeWidth: 4,
					color: "#FFEA82",
					percentage: 1.0,
					type: "line",
					sub: null
				}
			};
		},
		mounted: function () {
			console.log("crap");
			for (var key in this.field.options) {
				if (this.field.options.hasOwnProperty(key)) {
					this.options[key] = this.field.options[key];
				}
			}
			switch (this.options.type) {
				case "semi-circle":
					this.drawSemiCircle();
					break;

				default:
					this.drawLine();
					break;
			}
		},
		methods: {
			drawLine(options) {
				var bar = new ProgressBar.Line(this.$refs.chartContainer, {
					strokeWidth: this.options.strokeWidth,
					easing: "easeInOut",
					duration: 1400,
					color: this.options.color,
					trailColor: "#eee",
					trailWidth: 1,
					svgStyle: null,
					text: {
						style: {
							color: "#999",
							textAlign: "center",
							position: "relative",
							padding: 0,
							margin: 0,
							transform: null
						},
						autoStyleContainer: false
					},
					from: {color: this.options.fromColor},
					to: {color: this.options.toColor},
					step: (state, bar) => {
						bar.setText(Math.round(this.field.value * 100) + " %");

						if (this.options.animateColor && this.field.value <= 1) {
							bar.path.setAttribute("stroke", state.color);
						}

						if (this.field.value > 1) {
							bar.path.setAttribute("stroke", this.options.overflowColor);
						}
					}
				});

				bar.animate(this.field.value > 1 ? 1 : this.field.value);
			}
		}
	};
</script>
