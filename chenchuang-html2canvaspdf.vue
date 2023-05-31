<template>
	<view class="content">

		<view class="exportPdfBtn" @click="downKcReport()">导出PDF</view>

		<!-- 可视化 -->
		<view style="background-color: white; border-radius: 4px; margin-top: -4px;">

			<!-- 柱形图 堆叠 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px;"> 柱形图 堆叠
			</h4>
			<div class="mui-content-padded">

				<div id="dsj_zscq" style="width: calc(100vw - 40px); height: 280px;"></div>

			</div>


			<!-- 折线图 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px;"> 折线图
			</h4>
			<div class="mui-content-padded">

				<div id="dsj_cxcg" style="width: calc(100vw - 40px); margin: -18px 0px; height: 280px;">
				</div>

			</div>


			<!-- 柱状图 + 折线图-->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				柱状图 + 折线图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zlhjl" style="width: calc(100vw - 40px); height: 290px;"></div>

			</div>

			<!-- 有效知识产权结构 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				饼图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zscqjg" style="width: calc(100vw - 40px); height: 306px;"></div>





			</div>

			<!-- 专利iPC结构 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				树形结构图 </h4>
			<div class="mui-content-padded">



				<div id="dsj_zlipc_content"
					style="margin-left: -10px;  width: calc(100vw - 40px); height: auto; margin-bottom: 30px;">
				</div>







			</div>

			<!-- 柱状图 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				柱状图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zljz" style="width: calc(100vw - 40px); height: 280px;"></div>





			</div>

			<!-- 专利法律状态分布 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				饼图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zlfl" style="width: calc(100vw - 40px); height: 280px;"></div>


			</div>

			<!-- 专利寿命分布 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				柱状图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zlsm" style="width: calc(100vw - 40px); height: 280px;"></div>


			</div>

			<!-- 树形结构图 -->
			<h4 style="margin-left: 12px; font-size: 16px; height: 30px; line-height: 30px; margin-top: 0px;">
				树形结构图 </h4>
			<div class="mui-content-padded">

				<div id="dsj_zllhsq" style="width: calc(100vw - 40px); height: auto; margin-bottom: 30px;">
				</div>


			</div>

		</view>
	</view>
</template>

<script>
	import myIndex from '../index/index.js'

	export default {
		data() {
			return {
				title: 'Hello'
			}
		},
		mounted() {

			// 刷新Echart数据
			this.refreshEchartData();




		},

		onLoad() {

			//  页面加水印方法
			this.watermark({
				"watermark_txt": "中国广东cc.gd"
			});

		},
		methods: {

			//  页面加水印方法
			watermark(settings) {

				//默认设置
				var defaultSettings = {
					watermark_txt: "text",
					watermark_x: 20, //水印起始位置x轴坐标
					watermark_y: 80, //水印起始位置Y轴坐标
					watermark_rows: 80, //水印行数
					watermark_cols: 20, //水印列数
					watermark_x_space: 80, //水印x轴间隔
					watermark_y_space: 102, //水印y轴间隔
					watermark_color: '#aaa', //水印字体颜色
					watermark_alpha: 0.4, //水印透明度
					watermark_fontsize: '14px', //水印字体大小
					watermark_font: '微软雅黑', //水印字体
					watermark_width: 180, //水印宽度
					watermark_height: 80, //水印长度
					watermark_angle: 20 //水印倾斜度数
				};
				//采用配置项替换默认值，作用类似jquery.extend
				if (arguments.length === 1 && typeof arguments[0] === "object") {

					console.log("arguments = " + JSON.stringify(arguments[0]));
					// 获取参数配置
					var src = arguments[0];
					for (let key in src) {
						if (src[key] && defaultSettings[key] && src[key] === defaultSettings[key])
							continue;
						else if (src[key])
							defaultSettings[key] = src[key];
					}
				}

				var oTemp = document.createDocumentFragment();

				//获取页面最大宽度
				var page_width = Math.max(document.body.scrollWidth, document.body.clientWidth);
				var cutWidth = page_width * 0.0150;
				var page_width = page_width - cutWidth;
				//获取页面最大高度
				var page_height = Math.max(document.body.scrollHeight, document.body.clientHeight) + 2860;
				// var page_height = document.body.scrollHeight+document.body.scrollTop;
				//如果将水印列数设置为0，或水印列数设置过大，超过页面最大宽度，则重新计算水印列数和水印x轴间隔
				if (defaultSettings.watermark_cols == 0 || (parseInt(defaultSettings.watermark_x + defaultSettings
						.watermark_width * defaultSettings.watermark_cols + defaultSettings.watermark_x_space * (
							defaultSettings.watermark_cols - 1)) > page_width)) {
					defaultSettings.watermark_cols = parseInt((page_width - defaultSettings.watermark_x + defaultSettings
						.watermark_x_space) / (defaultSettings.watermark_width + defaultSettings
						.watermark_x_space));
					defaultSettings.watermark_x_space = parseInt((page_width - defaultSettings.watermark_x -
						defaultSettings
						.watermark_width * defaultSettings.watermark_cols) / (defaultSettings.watermark_cols - 1));
				}
				//如果将水印行数设置为0，或水印行数设置过大，超过页面最大长度，则重新计算水印行数和水印y轴间隔
				if (defaultSettings.watermark_rows == 0 || (parseInt(defaultSettings.watermark_y + defaultSettings
						.watermark_height * defaultSettings.watermark_rows + defaultSettings.watermark_y_space * (
							defaultSettings.watermark_rows - 1)) > page_height)) {
					defaultSettings.watermark_rows = parseInt((defaultSettings.watermark_y_space + page_height -
						defaultSettings
						.watermark_y) / (defaultSettings.watermark_height + defaultSettings.watermark_y_space));
					defaultSettings.watermark_y_space = parseInt(((page_height - defaultSettings.watermark_y) -
						defaultSettings
						.watermark_height * defaultSettings.watermark_rows) / (defaultSettings.watermark_rows - 1));
				}
				var x;
				var y;
				for (var i = 0; i < defaultSettings.watermark_rows; i++) {
					y = defaultSettings.watermark_y + (defaultSettings.watermark_y_space + defaultSettings
						.watermark_height) * i;
					for (var j = 0; j < defaultSettings.watermark_cols; j++) {
						x = defaultSettings.watermark_x + (defaultSettings.watermark_width + defaultSettings
								.watermark_x_space) *
							j;

						var mask_div = document.createElement('div');
						mask_div.id = 'mask_div' + i + j;
						mask_div.className = 'mask_div';
						mask_div.appendChild(document.createTextNode(defaultSettings.watermark_txt));
						//设置水印div倾斜显示
						mask_div.style.webkitTransform = "rotate(-" + defaultSettings.watermark_angle + "deg)";
						mask_div.style.MozTransform = "rotate(-" + defaultSettings.watermark_angle + "deg)";
						mask_div.style.msTransform = "rotate(-" + defaultSettings.watermark_angle + "deg)";
						mask_div.style.OTransform = "rotate(-" + defaultSettings.watermark_angle + "deg)";
						mask_div.style.transform = "rotate(-" + defaultSettings.watermark_angle + "deg)";
						mask_div.style.visibility = "";
						mask_div.style.position = "absolute";
						mask_div.style.left = x + 'px';
						mask_div.style.top = y + 'px';
						mask_div.style.overflow = "hidden";
						mask_div.style.zIndex = "9999";
						mask_div.style.pointerEvents = 'none'; //pointer-events:none  让水印不遮挡页面的点击事件
						//mask_div.style.border="solid #eee 1px";
						mask_div.style.opacity = defaultSettings.watermark_alpha;
						mask_div.style.fontSize = defaultSettings.watermark_fontsize;
						mask_div.style.fontFamily = defaultSettings.watermark_font;
						mask_div.style.color = defaultSettings.watermark_color;
						mask_div.style.textAlign = "center";
						mask_div.style.width = defaultSettings.watermark_width + 'px';
						mask_div.style.height = defaultSettings.watermark_height + 'px';
						mask_div.style.display = "block";
						oTemp.appendChild(mask_div);
					};
				};
				document.body.appendChild(oTemp);
			},

			// 下载pdf报告
			downKcReport() {


				html2canvas(
					document.body, {
						scale: 2,
						dpi: 192, //导出pdf清晰度
						onrendered: function(canvas) {
							var contentWidth = canvas.width;
							var contentHeight = canvas.height;

							//一页pdf显示html页面生成的canvas高度;
							var pageHeight = contentWidth / 592.28 * 841.89;
							//未生成pdf的html页面高度
							var leftHeight = contentHeight;
							//pdf页面偏移
							var position = 0;
							//html页面生成的canvas在pdf中图片的宽高（a4纸的尺寸[595.28,841.89]）
							var imgWidth = 595.28;
							var imgHeight = 592.28 / contentWidth * contentHeight;

							var pageData = canvas.toDataURL('image/jpeg', 1.0);
							var pdf = new jsPDF('', 'pt', 'a4');

							//有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
							//当内容未超过pdf一页显示的范围，无需分页
							if (leftHeight < pageHeight) {
								pdf.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight);
							} else {
								while (leftHeight > 0) {
									pdf.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight)
									leftHeight -= pageHeight;
									position -= 841.89;
									//避免添加空白页
									if (leftHeight > 0) {
										pdf.addPage();
									}
								}
							}
							pdf.save(name + '.pdf');
							console.log('成功导出pdf');

						},
						//背景设为白色（默认为黑色）
						background: "#fff"
					})

			},

			refreshEchartData() {

				// 返回数据需在json转换格式工具 转成Json格式
				let result = {
					"data": {
						"unionCompanyNum": 4,
						"goldList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"fmsqYx": "1",
							"cfmwyxzscqsl": "1",
							"years": "1",
							"yxzscqsl": "1",
							"dnzlhjl": "1"
						}],
						"patentLifeList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"percentage": "1",
							"quantity": "1",
							"scope": "1-3",
							"totalNum": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"percentage": "1",
							"quantity": "1",
							"scope": "3-5",
							"totalNum": "2"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"percentage": "1",
							"quantity": "3",
							"scope": "5-10",
							"totalNum": "3"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"percentage": "1",
							"quantity": "3",
							"scope": "10-",
							"totalNum": "4"
						}],
						"patentValueList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"fmgbZs": "1",
							"fmsqYx": "1",
							"syxxYx": "1",
							"wgsjYx": "1",
							"scope": "0-5",
							"totalNum": "1",
							"percentageHighRights": "1",
							"percentageHigh": "1",
							"dspw": "1",
							"jslypw": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"fmgbZs": "1",
							"fmsqYx": "1",
							"syxxYx": "1",
							"wgsjYx": "1",
							"scope": "5-10",
							"totalNum": "1",
							"percentageHighRights": "1",
							"percentageHigh": "1",
							"dspw": "1",
							"jslypw": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"fmgbZs": "1",
							"fmsqYx": "2",
							"syxxYx": "3",
							"wgsjYx": "4",
							"scope": "90-100",
							"totalNum": "10"
						}],
						"legalList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"underTrial": "1",
							"invalid": "1",
							"efficient": "1",
							"totalNum": "1",
							"percentage": "1"
						}],
						"propertyRightList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"fmgbZs": "1",
							"fmgbZsP": "1",
							"fmsqYx": "1",
							"fmsqYxP": "1",
							"syxxYx": "1",
							"syxxYxP": "1",
							"wgsjYx": "1",
							"wgsjYxP": "1",
							"rzYx": "1",
							"rzYxP": "1",
							"maxPercentage": "1",
							"maxPercentageRights": "1",
							"dspw": "1",
							"jslypw": "1"
						}],
						"unionPatentNum": 13,
						"ipcList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"ipcDept": "1",
							"ipcClass": "1",
							"ipcNum": "1",
							"ipcZbClass": "1",
							"ipcZb": "1",
							"dspw": "1",
							"jslypw": "1"
						}],
						"yearList": [{
							"isNewRecord": true,
							"comId": "1",
							"fmgbZs": "1",
							"fmsqYx": "1",
							"syxxYx": "1",
							"wgsjYx": "1",
							"rzYx": "1",
							"years": "2022",
							"dnkjcxcgkbjshl": "1",
							"socialCode": "123456"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"years": "2023",
							"dnkjcxcgkbjshl": "2",
							"socialCode": "123456"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"years": "2026",
							"dnkjcxcgkbjshl": "6",
							"socialCode": "123456"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"years": "2027",
							"dnkjcxcgkbjshl": "7",
							"socialCode": "123456"
						}],
						"unionList": [{
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"unionName": "测试5",
							"unionNum": "5",
							"unionCompany": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"unionName": "测试4",
							"unionNum": "4",
							"unionCompany": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"unionName": "测试3",
							"unionNum": "3",
							"unionCompany": "1"
						}, {
							"isNewRecord": true,
							"comId": "1",
							"socialCode": "123456",
							"unionName": "测试1",
							"unionNum": "1",
							"unionCompany": "1"
						}]
					},
					"statusCode": 200,
					"header": {
						"date": "Tue, 30 May 2023 07:08:28 GMT",
						"powered-by": "JeeSite V5.0.1 0",
						"transfer-encoding": "chunked",
						"content-type": "application/json"
					},
					"errMsg": "request:ok"
				};

				let resultData = result.data;

				if (resultData.yearList != undefined && resultData.yearList.length > 0) {
					console.log('执行图表1');
					// 填充图表1 2
					myIndex.fillChartOneTwo(resultData.yearList);

				}
				if (resultData.goldList != undefined && resultData.goldList.length > 0) {

					// 填充图表3
					myIndex.fillChartThree(resultData.goldList);

				}
				if (resultData.propertyRightList != undefined && resultData.propertyRightList.length > 0) {

					// 填充图表4
					myIndex.fillChartFour(resultData.propertyRightList);

				}
				if (resultData.ipcList != undefined && resultData.ipcList.length > 0) {

					// 填充图表5
					myIndex.fillChartFive(resultData.ipcList);

				}
				if (resultData.patentValueList != undefined && resultData.patentValueList.length > 0) {

					// 填充图表6
					myIndex.fillChartSix(resultData.patentValueList);

				}
				if (resultData.legalList != undefined && resultData.legalList.length > 0) {

					// 填充图表7 法律
					myIndex.fillChartSeven(resultData.legalList);

				}
				if (resultData.patentLifeList != undefined && resultData.patentLifeList.length > 0) {

					// 填充图表8 寿命
					myIndex.fillChartEight(resultData.patentLifeList);

				}
				if (resultData.unionList != undefined && resultData.unionList.length > 0) {

					// 填充图表9 专利联合
					myIndex.fillChartNine(resultData, "广汽集团");

				}
			}

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

	}

	.mui-content-padded {
		margin: 10px 8px;
	}

	.mui-content-padded div {

		font-size: 13px;
	}

	.exportPdfBtn {

		width: 100px;
		height: 40px;
		line-height: 40px;
		margin-top: 20px;
		background-color: greenyellow;
		text-align: center;
	}
</style>