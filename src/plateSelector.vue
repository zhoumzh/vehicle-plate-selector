<template>
    <div class="vehicle-plate" style="">
        <div>
            <div class="input_outer">
                <label class="label_province" for="id_province">
                    <input class="input_province" type="text" readonly="readonly" maxlength="1" id="id_province" @click="showProvince" v-model="selectedProvince"/>
                </label>
                <input id="id_plate0" class="input_platenumber_base" style="left:45px;" @keyup="writeP(0,$event)" @focus="toggleStyle(0,true)" @blur="toggleStyle(0,false)" maxlength="1" v-model="pvArray[0]"/>
                <input id="id_plate1" class="input_platenumber_base" style="left:81px;" @keyup="writeP(1,$event)" @focus="toggleStyle(1,true)" @blur="toggleStyle(1,false)" maxlength="1" v-model="pvArray[1]">
                <input id="id_plate2" class="input_platenumber_base" style="left:117px;" @keyup="writeP(2,$event)" @focus="toggleStyle(2,true)" @blur="toggleStyle(2,false)" maxlength="1" v-model="pvArray[2]">
                <input id="id_plate3" class="input_platenumber_base" style="left:153px;" @keyup="writeP(3,$event)" @focus="toggleStyle(3,true)" @blur="toggleStyle(3,false)" maxlength="1" v-model="pvArray[3]">
                <input id="id_plate4" class="input_platenumber_base" style="left:189px;" @keyup="writeP(4,$event)" @focus="toggleStyle(4,true)" @blur="toggleStyle(4,false)" maxlength="1" v-model="pvArray[4]">
                <input id="id_plate5" class="input_platenumber_base" style="left:225px;" @keyup="writeP(5,$event)" @focus="toggleStyle(5,true)" @blur="toggleStyle(5,false)" maxlength="1" v-model="pvArray[5]">
            </div>
        </div>
        <el-dialog append-to-body="true" title="请选择" :visible.sync="dialogVisible">
            <el-button type="text" v-for="one in provinces" @click="choose(one)">{{one}}</el-button>
        </el-dialog>
    </div>
</template>
<script>
    import $ from 'jquery'

    export default {
        name: "vehicle-plate-selector",
        data() {
            return {
                dialogVisible: false,
                selectedProvince: '冀',
                inputScope: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9,
                    'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I',
                    'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S',
                    'T', 'U', 'V', 'W', 'X', 'Y', 'Z'],
                pvArray: [],
                provinces: ["京", "沪", "浙", "苏", "粤", "鲁", "晋", "冀",
                    "豫", "川", "渝", "辽", "吉", "黑", "皖", "鄂",
                    "津", "贵", "云", "桂", "琼", "青", "新", "藏",
                    "蒙", "宁", "甘", "陕", "闽", "赣", "湘"]
            };
        },
        props: ['value'],
        methods: {
            showProvince() {
                this.dialogVisible = true;
                $(".input_platenumber_base").each(function () {
                    this.blur();
                });
            },
            toggleStyle: function (id, b) {
                if (b) {
                    $('#id_plate' + id).addClass('focus_style');
                } else {
                    $('#id_plate' + id).removeClass('focus_style');
                }
                this.updateVal();
            },
            choose(p) {
                this.selectedProvince = p;
                this.dialogVisible = false;
                $('#id_plate0').focus();
            },
            writeP(id, key) {
                const k_del = 8, k_enter = 13, k_left = 37, k_right = 39;
                const k0 = 48, k9 = 57, ka = 65, kz = 90, kk = 7;
                if (key.keyCode >= k0 && key.keyCode <= kz) {
                    let ai = key.keyCode - k0;
                    if (key.keyCode >= ka) {
                        //字母段再减去中间数字和字母中间的空位
                        ai -= kk;
                    } else if (id === 0) {
                        //数字段在城市框也不可输入
                        this.pvArray.splice(id, 1, '');
                        return;
                    }
                    this.pvArray.splice(id, 1, this.inputScope[ai]);
                    $('#id_plate' + (id + 1)).focus();
                    if (id === 5) {
                        $("#id_plate5").blur();
                    }
                } else if (key.keyCode === k_del) {
                    this.pvArray.splice(id, 1, '');
                    $('#id_plate' + (id - 1)).focus();
                    if (id === 0) {
                        this.showProvince();
                    }
                } else if (key.keyCode === k_left) {
                    $('#id_plate' + (id - 1)).focus();
                } else if (key.keyCode === k_right) {
                    $('#id_plate' + (id + 1)).focus();
                } else if (key.keyCode === k_enter) {
                    $(".input_platenumber_base").each(function () {
                        this.blur();
                    });
                }
                else {
                    let cv = this.pvArray[id];
                    if (/^\w$/.test(cv)) {
                        this.pvArray.splice(id, 1, cv.toUpperCase());
                    } else {
                        this.pvArray.splice(id, 1, '');
                    }
                }
            },
            getPlate() {
                return this.selectedProvince + this.pvArray.join('');
            },
            updateVal: function () {
                // 2、手动触发父组件的input事件并将值传给父组件
                this.$emit('input', this.getPlate());
            }
        }
    }

</script>
<style>
   .vehicle-plate {

   }

   .input_outer {
       height: 40px;
       width: 100%;
       position: relative;
       top: 0;
   }

   .input_province {
       position: absolute;
       top: 0;
       left: 9px;
       text-align: center;
       width: 34px;
       height: 34px;
       border-left: 1px solid #c5c5c5;
       border-top: 1px solid #c5c5c5;
       border-bottom: 1px solid #c5c5c5;
       outline: 0;
       font-weight: bold;
       font-family: "黑体";
       color: darkblue;
       /*#0861fc*/
   }

   .input_outer .input_platenumber_base:last-child {
       border-right: 1px solid #c5c5c5;
   }

   .input_platenumber_base {
       position: absolute;
       text-align: center;
       width: 34px;
       height: 34px;
       top: 0;
       border-left: 1px dashed #c5c5c5;
       border-top: 1px solid #c5c5c5;
       border-bottom: 1px solid #c5c5c5;
       outline: 0;
       font-family: "黑体";
       font-weight: bold;
   }

   .focus_style {
       /*已获得焦点样式*/
       border: 1px solid rgba(82, 168, 236, .8);
       border-radius: 2px;
       box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
       -webkit-box-shadow: inset 0px 2px 2px rgba(0, 0, 0, 0.75), 0 0 8px rgba(82, 168, 236, 0.6);
   }
</style>
