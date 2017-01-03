
### 关于velocity
Velocity专为动画而设计 ，简单易用，功能强大，广泛地被一些主流公司所使用（包括Tumblr, Microsoft and WhatsApp）。并且Velocity 是基于MIT许可协议的开源库。
Velocity模仿了jQuery的语法，可以完美地同jQuery协作，当然也能独立地使用，所以对你来说应该很容易学。由于jQuery使用普遍，我也将向你展示Velocity如何同它协作。

### 关于svg
这个就不陌生了，这是一个矢量的标签元素，好吧，还是把百科的弄过来，可缩放矢量图形是基于可扩展标记语言（标准通用标记语言的子集），用于描述二维矢量图形的一种图形格式。它由万维网联盟制定，是一个开放标准。

### 说明

这是一个个人网站主页的展示案例，利用svg的路径节点与velocity进行制作的动画序列，可以完好的按照一定的时间对path进行相应的动画，看起来是挺酷的，比如常见的路径跟随效果是非常容易完成的一件事

### 案例地址

 [访问地址](http://tanxu.top)

### 案例代码
<code>
   (function($){

	//动画共有方法
	function createSchoolAnimate(){
		//自定义的动画类型， 路径跟随动画
		 $.Velocity.RegisterUI('slidePath',{
	        defaultDuration:400,
	        calls:[
	          [{strokeDashoffset:0}]
	        ]
	    });
	}
	//批量生成动画序列，返回数组
	function createAnimateSeq(objName){
	    	var arr = [];
	    	for(var i=0; i<objName.length;i++){
	    		var object = {};
	    		object.elements = $('#'+objName+'_'+i+'');
	    		object.properties = "slidePath";
	    		object.options = {
	    			duration:200
	    		}
	    		arr.push(object);
	    	}
			return arr;
	}
	//统一设置stroke-dasharray
	function setStroke(objName,stringName){
		for(var i=0; i<objName.length;i++){
		          objName[i].style.strokeDasharray = objName[i].getTotalLength();
		          objName[i].style.strokeDashoffset = objName[i].getTotalLength();
		          objName[i].id= stringName+'_'+i;
		}
	}
	
	$(function(){
	//判定移动端或者PC端
	if(IsPC()){
		$(window).resize(function(){
			if($(window).width()<960){
				window.location.href = './pc_min.html';
			}
		})
	}else{
		window.location.href = './index_m.html';
	}
	function IsPC(){  
		var userAgentInfo = navigator.userAgent;  
		var Agents = new Array("Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod");  
		var flag = true;  
		for (var v = 0; v < Agents.length; v++) {  
		  if (userAgentInfo.indexOf(Agents[v]) > 0) { flag = false; break; }  
		}  
		return flag;  
		}
		
	});
	
	
	
	
	
	//slidebar动画序列
	var sidebarFn = {
		elements:{
			closeBtn:$('.close_slidebar'),
			openBtn : $(".open_sidebar"),
			sideBar :$('#slidebar'),	
			humanImg : $('.img_wrap'),
			humanInfo : $('.human_info'),
			nameTitle:$('#slidebar h2'),
			humanDesc:$("#slidebar>p"),
			closeSpan:$("#close_sidebar")
		},
		getSeq:function(){
			var _this = this;
			var seq = [
				        //第一阶段
				        {
				          elements:_this.elements.sideBar,
				          properties:{
				             left:[0,-283]
				          },
				          options:{duration:200}
				        },
				         //第二阶段
				        {
				          elements:_this.elements.humanImg,
				          properties:{
				             marginTop:'20px'
				          },
				          options:{duration:150,delay:150}
				        },
				         //第二阶段
				        {
				          elements:_this.elements.nameTitle,
				          properties:{
				             opacity:1
				          },
				          options:{duration:200,delay:100}
				        },
				         //第二阶段
				        {
				          elements:_this.elements.humanDesc,
				          properties:{
				             opacity:1
				          },
				          options:{duration:200,delay:100}
				        },
				        //第二阶段
				        {
				          elements:_this.elements.humanInfo,
				          properties:{
				             opacity:1
				          },
				          options:{duration:500,delay:100}
				        }
				    ]
			//
			return seq;
		},
		init: function(){
			var _this = this;
			
			_this.elements.openBtn.on('click',function(){
				 $.Velocity.RunSequence(_this.getSeq());
			});
			_this.elements.closeBtn.on('click',function(){
				_this.elements.sideBar.velocity({
					left : -283
				},{
					duration:200
				})
			})
		}
	}
	sidebarFn.init();
	//页面布局
	var pageBuild = {
		elements:{
			main: $("#main"),
			prevBtn:$(".gotoPage .prev"),
			nextBtn:$('.gotoPage .next'),
			nav:$('#nav'),
			gotoNext:$('.page1 .text_desc .gotoMore')
		},
		init : function(){
			/*控制台输出*/
			console.log('啊哦！你好像试图查看我的页面元素...');
			//如果是小屏幕，跳转到提示页
			
			
			
			var _this = this;
			_this.elements.main.onepage_scroll({
				sectionContainer: '.page',
				direction: 'horizontal',
				pagination:  false,
				esing: 'ease-in',
				updateURL: false,  
				animationTime: 800,
				responsiveFallback: false,
				beforeMove: function(index) {
					// 页面滚动前回调函数
				    if(index == 2){
				    	//第二屏幕
				    	 mySkill();  	
				    }else if(index == 3){
				    	//第三屏
				    	myWork();
				    }else if( index == 4){
				    	myPen();
				    }else if(index == 1 ){
				    	//pageBuild.init();
				    }
				},  
   				afterMove: function(index) {
   					//第二屏幕
				  
   				}// 页面滚动后回调函数
			});
			//
			this.eventFn();
			this.initAnimation();
		},
		eventFn: function(){
			var _this = this;
			_this.elements.prevBtn.on('click',function(){
				_this.elements.main.moveUp();
			});
			_this.elements.nextBtn.on('click',function(){
				_this.elements.main.moveDown();
			});
			_this.elements.gotoNext.on('click',function(){
				_this.elements.main.moveDown();
			});
			
			//导航
			var nav = _this.elements.nav;
			nav.find('li:not(".humanInfo")').on('click',function(){
				_this.elements.main.moveTo($(this).index()); //滚动到index值的页面
			});
		},
		initAnimation: function(){
			var  _this = this;
			var gotoNext = _this.elements.gotoNext;
			//往前的小动画
			gotoNext.velocity({
				right:25
			},{
				duration:1000,
				loop:true
			});
			//执行创建自定义动画
			createSchoolAnimate();
			//进来时候的动画
			var mySchool = $("#mySchool path");
			var myTree = $(".my_tree path");
			//统一设置path的stroke-dasharray
			setStroke(mySchool,'mySchool');
			setStroke(myTree,'myTree');
		    //创建动画序列数组
		    //var objectArr = createAnimateSeq(mySchool);
		    //直线动画
			var seq = [
				{
					elements: $(".page1 .drawing"),
					properties:{
						width:$("#mySchool g").offset().left
					},
					options:{
						duration:500
					}
				},
				{
					elements:$("#mySchool_0"),
					properties: 'slidePath'
				},
				{
					elements:$("#mySchool_1"),
					properties: 'slidePath'
				},
				{
					elements:$("#mySchool_2"),
					properties: 'slidePath'
				},
				{
					elements:$("#mySchool_3"),
					properties: 'slidePath'
				},
				{
					elements:$("#mySchool_4"),
					properties: 'slidePath',
					options:{
						duration:100
					}
				},
				{
					elements:$("#mySchool_5"),
					properties: 'slidePath',
					options:{
						duration:100
					}
				},
				{
					elements:$("#mySchool_6"),
					properties: 'slidePath',
					options:{
						duration:100
					}
				},
				{
					elements:$("#mySchool_7"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_8"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_9"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_10"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_11"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_12"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_13"),
					properties: 'slidePath'
				}
				,
				{
					elements:$("#mySchool_14"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				}
				,
				{
					elements:$("#mySchool_15"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements:$("#mySchool_16"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements:$("#mySchool_17"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements:$("#mySchool_18"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements:$("#mySchool_19"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},{
					elements:$(".page1 .cloud"),
					properties:{
						opacity:1,
					}
				},
				{
					elements:$(".page1 .cloud01"),
					properties:{
						top:160
					},
					options:{
						loop:true,
						sequenceQueue:false,
						duration:2600
					}
				},
				{
					elements:$(".page1 .cloud02"),
					properties:{
						top:167
					},
					options:{
						loop:true,
						delay: 200,
						sequenceQueue:false,
						duration:2400
					}
				},
				{
					elements:$(".page1 .text_desc"),
					properties:{
						opacity:1
					},
					options:{
						sequenceQueue:false,
						duration:500,
						easing:'ease-in'
					}
				},
				{
					elements: $(".page1 .drawing"),
					properties:{
						width:[$(".page .my_tree svg path:first-child").offset().left,$("#mySchool g").width()+$('#mySchool g').offset().left]
					},
					options:{
						duration:500
					}
				},
				{
					elements: $("#myTree_0"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements: $("#myTree_1"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements: $("#myTree_2"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements: $("#myTree_3"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements: $("#myTree_4"),
					properties: 'slidePath',
					options:{
						duration:200
					}
				},
				{
					elements: $(".page1 .drawing"),
					properties:{
						width:[$(window).width(),$(".page1 .my_tree svg").width()+$('.page1 .my_tree').offset().left]
					},
					options:{
						duration:500,
						complete :function(){
							//第一页完成的回调
							
						}
					}
				}
			];
			$.Velocity.RunSequence(seq);
		}
	}
	pageBuild.init();
	
	//第二屏幕
	
	var state = 0;
	function mySkill(){
		if(state == 0){
			var mySkill = $("#skills path");

			//统一设置path的stroke-dasharray
			setStroke(mySkill,'mySkill');
			var seq1 = [
						{
							elements: $(".page2 .drawing"),
							properties:{
								width:[($("#mySkill_0").width()/2)+($("#mySkill_0").offset().left-$(window).width()+250),0]
							},
							options:{
								duration:1500
							}
						},
						{
							elements:$("#mySkill_0"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_1"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_2"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_3"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_4"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_5"),
							properties: 'slidePath',
							options:{
								duration:200
							}
							
						},
						{
							elements:$("#mySkill_5"),
							properties: 'slidePath',
							options:{
								duration:200
							}
						},
						{
							elements:$("#mySkill_6"),
							properties: 'slidePath',
							options:{
								duration:200
							}
						},
						{
							elements:$("#mySkill_7"),
							properties: 'slidePath',
							options:{
								duration:200
							}
						},
						{
							elements:$("#mySkill_8"),
							properties: 'slidePath',
							options:{
								duration:200
							}
						},
						{
							elements:$("#mySkill_9"),
							properties: 'slidePath'
						},
						{
							elements:$("#mySkill_10"),
							properties: 'slidePath'
						},
						{
							elements: $(".page2 .drawing"),
							properties:{
								width:[$(window).width(),($("#mySkill_0").width()/2)+($("#mySkill_0").offset().left-$(window).width()+250)]
							},
							options:{
								duration:1500
							}
						},
						{
							elements:$(".page2 .text_desc"),
							properties:{
								opacity:1
							},
							options:{
								duration:500
							}
						},
						{
							elements:$(".page2 .label_wrap"),
							properties:{
								opacity:1
							},
							options:{
								duration:500
							}
						},
						{
							elements: $('.page2 .ract_01,.page2 .ract_04'),
							properties:{
								top:155,
								opacity:1
							},
							options:{
								duration:1800,
								easing:'easeInSine'
							}
						},
						{
							elements: $('.page2 .ract_02,.page2 .ract_05'),
							properties:{
								top:155,
								opacity:1
							},
							options:{
								duration:1800,
								easing:'easeInSine'
							}
						},
						{
							elements: $('.page2 .ract_03,.page2 .ract_06'),
							properties:{
								top:155,
								opacity:1
							},
							options:{
								duration:1800,
								easing:'easeInSine',
								delay:200
							}
						}
						
			];
			$.Velocity.RunSequence(seq1);
			state = 1;
		}
	  	//箭头小动画
	  	$(".page2 .gotoMore").velocity({
	  		right:[102,130]
	  	},{
	  		loop:true,
	  		duration:1500
	  	});
	  	$(".page2 .gotoMore").on('click',function(){
	  		$("#main").moveDown();
	  	});
	}
	
	//第三屏
	var state03 = 0;
	function myWork(){
		var el = new Object();
		el.dai = $("#dai path");
		el.drawLine = $(".page3 .drawing");
		
		//自定义动画
		 $.Velocity.RegisterUI('daislide',{
	        defaultDuration:2300,
	        calls:[
	          [{top:-14,opacity:1},0.4],
	          [{left:380},0.3],
	          [{opacity:0},0.1,{loop:true}],
	          [{opacity:1},0.1,{loop:true}]
	        ]
	    });
		
		//
		if(state03 == 0){
			var myWork = el.dai;

			//统一设置path的stroke-dasharray
			setStroke(myWork,'myWork');
			var seq3 = [
				{
					elements : $('.page3 .drawing'),
					properties:{
						width : [$("#dai").offset().left-$(window).width()+$("#dai").width()-15,0]
					},
					options:{
						duration:500
					}
				},
				{
					elements:$("#myWork_0"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_1"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_2"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_3"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_4"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_5"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_6"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_7"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_8"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_9"),
					properties:"slidePath"
				},
				{
					elements:$("#myWork_10"),
					properties:"slidePath"
				},
				{
					elements:$(".page3 .text_desc"),
					properties:{
						opacity:1
					},
					options:{
						duration:500
					}
				},
				
				{
					elements:$(".page3 .label_wrap"),
					properties:{
						opacity:1
					},
					options:{
						duration:500
					}
				},
				{
					elements :$(".page3 .smoke"),
					properties:"slidePath",
					options:{
						//sequenceQueue:false
					}
				},
				{
					elements : $('.page3 .drawing'),
					properties:{
						width : [$(window).width(),$("#dai").offset().left-$(window).width()+$("#dai").width()-15]
					},
					options:{
						duration:500
					}
				},
				{
					elements: $(".page3 .dai_item_01"),
					properties: "daislide",
					options:{
						loop:true
					}	
				},
				{
					elements: $(".page3 .dai_item_02"),
					properties: "daislide"
				},
				{
					elements :$(".page3 .smoke"),
					properties:{
						bottom:300,
						scale:1.2
					},
					options:{
						loop:true,
						duration:2100
					}
				}
				
			]
			$.Velocity.RunSequence(seq3);
			state03 = 1;
		}
	}
	
	//第四屏
	var state04 = 0;

	function myPen(){
		
		
		if(state04 == 0){
			var myPen = $("#myPen path");
	    	setStroke(myPen,'myPen');
			var seq4 = [
			   {
					elements : $('.page4 .drawing'),
					properties:{
						width : [$("#myPen").offset().left-$(window).width(),0]
					},
					options:{
						duration:500
					}
				},
				{
					elements:$("#myPen_3"),
					properties:"slidePath"
				},
				{
					elements:$("#myPen_4"),
					properties:"slidePath"
				},
				{
					elements:$("#myPen_2"),
					properties:"slidePath"
				},
				{
					elements:$("#myPen_1"),
					properties:"slidePath"
				},
				{
					elements:$("#myPen_0"),
					properties:"slidePath"
				},
				{
					elements : $(".page4 .text_desc"),
					properties :{
						opacity:1
					},
					options:{
						duration:300
					}
				},
				{
					elements : $(".reward_ewm"),
					properties :{
						opacity:1
					},
					options:{
						duration:300
					}
				}
				]
			state04  = 1;
			$.Velocity.RunSequence(seq4);
			
		}else {
			return ;
		}
	}
})(jQuery)

</code>   
   
   
