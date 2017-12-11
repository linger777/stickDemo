# stickDemo
sticky demo project 

一、场景

需求：tab需要在划出视口的时候吸顶（sticky），方便点击切换下方内容。

二、方案
1、采用scroll监听＋fixed定位。
.fixed-top{
    position: fixed;
    left:0;
    top:0; //num
    z-index:10;
}
a) 监听页面被卷高度scrollTop，和 要sticky的元素距离页面的高度offsetTop，如果前者大于后者，设置fixed样式。
$(window).scroll(function(){
    var $stickyEl = $('.sticky');
    var scrollT = $(window).scrollTop();
    if (scrollT > (_stickyT-num)) { //num是吸顶时距离viewport顶部的距离
        $stickyEl.addClass('fixed-top');
    } else {
        $stickyEl.removeClass('fixed-top');
    }
});
b)通过getBoundingClientRect()监听要sticky元素的位置top 和0，stickyHeight和bottom进行比较，如果前者小于后者，设置fixed样式。
$(window).scroll(function() {
    var $stickyEl = $('.sticky');
    var rect = $stickyEl.getBoundingClientRect();
    if(rect.top < 0 && (rect.bottom - stickyHeight) > 0) {
        !$stickyEl.hasClass('fixed-top') && $stickyEl.addClass('fixed-top').css('width', stickyWidth +'px');
    }else{
        $stickyEl.hasClass('fixed-top') && $stickyEl.removeClass('fixed-top').css('width','auto');
    }
});
2、采用position:sticky定位。
.sticky-top{
    position: -webkit-sticky;
    position: sticky;
    top:0;
    left:0;
    z-index:10;
}

三、position : sticky
粘性定位是相对定位和固定定位的混合。元素在跨越特定阈值(例如，从视口顶部10像素）前为相对定位，之后为固定定位。
须指定top,right,bottom或left四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

四、整体实现思路：
（1）采用sticky优先的原则，判断是否支持sticky。如果支持，采用方案2；否则采用方案1-a。
（2）在使用方案1-a时，滚动实时监听会带来频繁的调用回调函数，引来性能问题，在此采用函数截流的方式解决；
（3）在接触到阀值临界时，被sticky元素会有跳动，采用守家占位方案解决。

