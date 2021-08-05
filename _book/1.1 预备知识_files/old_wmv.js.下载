/*专门针对原来old_wmv的程序,需要JQuery支持*/
var __HX_PopupLearnAlert=null;
$(document).ready(function(){
	/*消除表格的100%问题*/
	$("div>table[width='100%']").each(function(){
		var t=$(this);
		t.attr("width","95%");
	});
	/*提示的特出框处理*/
	$("body").append('<div id="popup-win"><div class="head"><span><img src="__templates/htmlv1/images/old_wmv/window_close_btn.gif" onclick="__HX_HideLearnAlert()" style="cursor:pointer;"/></span>学习提示</div><iframe id="popup-win-content" class="content"></iframe></div>');
	__HX_PopupLearnAlert=$("#popup-win").hide();
	$("a[Act=click_open]").each(function(){
		$(this).click(function(){
			__HX_ShowLearnAlert($(this));
		});
	});
	$(".tab").css("display","none");
	$("a[Act=sh]").each(function(){
		$(this).click(function(){
			var tableId=$(this).attr("Elm");
			var table =  $("#"+tableId);
			var isDisplay = table.css("display");
			if(isDisplay=="none"){
				table.show();
			}else{
				table.hide();
			}
		});
	});
	var fls = __flashChecker();
	if(!fls.f){
		var flashObject=$('object[classid="clsid:D27CDB6E-AE6D-11cf-96B8-444553540000"]');
		//alert(flashObject.length);
		$("object[classid=clsid:D27CDB6E-AE6D-11cf-96B8-444553540000]").each(function(){
				$(this).after('<p style="color:#999999;padding:20px 10px 20px 10px;background-color:#f3f3f3;text-indent:0px;font-size:12px;text-align:center;font-weight: normal;">没有安装flash播放器,请在电脑上学习。</p>');
				$(this).remove();
		});
	}
});
function __HX_ShowLearnAlert(t){
	if(__HX_PopupLearnAlert!=null){
		var wh=t.attr("Wh");
		if(wh==null||wh==undefined||wh==""){
			wh=t.attr("wh");
		}
		var whs=wh.split(",");
		__HX_PopupLearnAlert.width(parseInt(whs[0]));
		__HX_PopupLearnAlert.height(parseInt(whs[1]));
		var c=$("#popup-win-content",__HX_PopupLearnAlert);
		c.height(__HX_PopupLearnAlert.height()-22);
		c.width(__HX_PopupLearnAlert.width()-4);
		var offset=t.offset();
		var win=$(window);
		var l=win.width()-__HX_PopupLearnAlert.width();
		if(offset.left<l){
			l=offset.left;
		}
		
		var h = offset.top+__HX_PopupLearnAlert.height()-$(document).scrollTop();
		if(h>$(window).height()){
			h = offset.top-__HX_PopupLearnAlert.height()-5;
		}else{
			h = offset.top+5;
		}
		__HX_PopupLearnAlert.css({"top":h,"left":(l-5)});
		__HX_PopupLearnAlert.show();
		var tt = t.attr("Elm");
		if(tt==null||tt==undefined||tt==""){
			tt=t.attr("elm");
		}
		c.attr("src",tt+"");
	}
}
function __flashChecker () {
	var hasFlash = 0; // 是否安装了flash
	var flashVersion = 0; // flash版本

	if (document.all) {
		var swf = new ActiveXObject('ShockwaveFlash.ShockwaveFlash');
		if (swf) {
				hasFlash = 1;
				VSwf = swf.GetVariable("$version");
				flashVersion = parseInt(VSwf.split(" ")[1].split(",")[0]);
		}
	} else {
		if (navigator.plugins && navigator.plugins.length > 0) {
			var swf = navigator.plugins["Shockwave Flash"];
			if (swf) {
				hasFlash = 1;
				var words = swf.description.split(" ");
				for (var i = 0; i < words.length; ++i) {
					if (isNaN(parseInt(words[i]))) continue;
					flashVersion = parseInt(words[i]);
				}
			}
		}
	}
	return {f:hasFlash,v:flashVersion};
}
function __HX_HideLearnAlert(){
	if(__HX_PopupLearnAlert!=null){
		__HX_PopupLearnAlert.hide();
	}
}