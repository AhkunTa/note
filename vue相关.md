#Vue 相关

###1 组件内实现多选数组
		<div class="chooser">
  		<ul class="chooser-list">
   			<li :style="cssStyle"
		    v-for="(item, index) in options" :key="index"
		    @click="optionsClick(item)"
		    :class="{active: checkActive(item)}"
		    >{{ item }}
		    ></li>
 	 	</ul>
	</div>
    


	export default {
	  	name: 'options',
	  	data () {
	    return {
	       currValArr: []
	    }
	  	},
	  	props: {
	   		options: Array, //传入的数组
	    	isMultiply: { //是否是多选。默认为false：单选；true：多选
	      	type: Boolean,
	      	default: false
	    },
	    cssStyle: Object //可以自定义单选或者多选的样式
	  },
		methods:{
			optionsClick (item) {
		      if (this.isMultiply === false) { //单选
		        this.$set(this.currValArr, 0, item) // 将该值设为当前数组的第一项
		      } else { //多选
		        if (this.currValArr.indexOf(item) === -1) {
		          // 当前数组中没有该值则push到数组
		          this.currValArr.push(item)
		        } else {
		          //当前数组中有该值，找到该值下标并删除
		          this.currValArr.splice(this.currValArr.indexOf(item), 1)
		        }
		      }
			},
			checkActive (item) {
		      if (this.isMultiply === false) {
		        this.currValArr.length = 1
		      }
		      return this.currValArr.indexOf(item) !== -1
			}
		}

-------------------------

###2 单选
	  <li class="select"
          v-for="(item,index) in leftnav"
          :class="{active:index == isA }"
          @click="ClickActive(index)"
      >{{item.name}}
      </li>


	data() {
      return {
        leftnav: [
          {name: '首页'},
          {name: '它的'},
          {name: '你的'}
        ],
        isA:0  //初始化第一个高亮
      }
    },
    methods:{
      ClickActive(index){
           this.isA = index
      }
    }

---------------------


###3 vue 防抖和节流事件解绑

	created(){
		this.handle = throttle(this.handleScroll,200)
	}
	beforeDestroy() {
		this.$nextTick(function(){
			window.addEventListener("scroll",this.handle)
		})
	}