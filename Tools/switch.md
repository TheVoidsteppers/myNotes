```html
<div class="multi-switch" style="margin: 0 auto;">
    <div class="switch-content" :class="{'disable':activity.is_stopsupply == '0','active':activity.is_stopsupply == '1'}"
         @click="changeSupply(activity.id,activity.is_stopsupply)">
        <div class="switch-circle"></div>
    </div>
    <input type="checkbox" value="0">
</div>
```

```css
.multi-switch{width:50px;user-select:none;-webkit-backface-visibility:hidden;-webkit-touch-callout:none;-webkit-user-select:none;-khtml-user-select:none;-moz-user-select:none;-ms-user-select:none}
.multi-switch *{-webkit-transition:all ease 0.3s;-moz-transition:all ease 0.3s;-ms-transition:all ease 0.3s;-o-transition:all ease 0.3s;transition:all ease 0.3s}
.multi-switch .switch-content{background:none;background-color:#D97C6F;height:31px;position:relative;cursor:pointer;border-radius:31px   ;-moz-border-radius:31px   ;-webkit-border-radius:31px   ;-ms-border-radius:31px   }
.multi-switch .switch-content .switch-circle{background:#FFF;width:29px;height:29px;position:absolute;top:1px;left:0%;z-index:1;margin-left:1px;border-radius:29px   ;-moz-border-radius:29px   ;-webkit-border-radius:29px   ;-ms-border-radius:29px   ;box-shadow: 3px 3px 0px rgba(0,0,0,0.1)  ;-moz-box-shadow: 3px 3px 0px rgba(0,0,0,0.1)  ;-webkit-box-shadow: 3px 3px 0px rgba(0,0,0,0.1)  }
.multi-switch .switch-content .info-slide{position:absolute;z-index:2;width:50%;height:100%;display:block}
.multi-switch .switch-content .info-slide.active{right:0;border-radius:0 31px 31px 0;-moz-border-radius:0 31px 31px 0;-webkit-border-radius:0 31px 31px 0;-ms-border-radius:0 31px 31px 0}
.multi-switch .switch-content .info-slide.disable{left:0;border-radius:31px 0 0 31px;-moz-border-radius:31px 0 0 31px;-webkit-border-radius:31px 0 0 31px;-ms-border-radius:31px 0 0 31px}
.multi-switch .switch-content.active{background-color:#5DC177}
.multi-switch .switch-content.active .switch-circle{left:100%;margin-left:-30px}
.multi-switch .switch-content.disabled{background-color:#CCC;cursor:default}
.multi-switch .switch-content.initial{background-color:#dddddd}
.multi-switch .switch-content.initial .switch-circle{left:50%;margin-left:-14.5px}
.multi-switch input{display:none}
```

