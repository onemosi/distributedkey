<template>
    <div>
        <div class="header">分布式密钥</div>
        <div class="w">
            <div class="left-box">
                <el-form label-position="left" label-width="80px">
                    <el-row :gutter="20" class="elrow">
                        <el-form-item label="密码：">
                            <el-input size="medium" v-model="form.plainText" placeholder="请输入密码"></el-input>
                        </el-form-item>
                        <el-form-item label="门限：">
                            <el-input size="medium" v-model="form.minDoor" placeholder="请输入门限"></el-input>
                        </el-form-item>
                        <el-form-item label="服务器：">
                            <el-input size="medium" v-model="form.maxDoor" placeholder="请输入服务器数量"></el-input>
                        </el-form-item>
                        <el-form-item label="操作：">
                            <el-button type="primary" size="medium" @click="encode">提交</el-button>
                        </el-form-item>
                    </el-row>
                </el-form>
            </div>
            <div class="right-box">
                <div class="floor">密钥分片</div>
                <div class="floor">
                    <div class="temp" v-for="(item,index) in sharding" :key="index">{{ item }}</div>
                </div>
                <div class="floor">选择分片</div>
                <div class="floor">
                    <el-select style="width:600px" v-for="(item,index) in dks" v-model="dks[index]" :key="index" size="medium">
                        <el-option v-for="(temp,index) in sharding" :key="index" :value="temp" :label=" temp"></el-option>
                    </el-select>
                </div>
                <div class="floor">解密结果:{{ word }}</div>
                <el-button type="primary" size="medium" @click="decode">解密</el-button>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'HelloWorld',
    data() {
        return {
            form: {
                plainText: '',
                minDoor: '',
                maxDoor: '',
            },
            sharding: [],
            dks: [],
            word: '',
        }
    },
    methods: {
        encode() {
            this.$request({
                url: '/encode',
                method: 'post',
                data: this.form
            }).then(response => {
                this.sharding = response.data;
                for (let i = 0; i < this.form.minDoor; i++) {
                    this.dks.push('');
                }
            })
        },
        decode() {
            this.$request({
                url: '/decode',
                method: 'post',
                data: this.dks
            }).then(response => {
                this.word = response.data.key;
            })
        }
    }
}
</script>

<style scoped>

* {
    margin: 0;
    padding: 0;
}

.header {
    height: 50px;
    background-color: #313132;
    color: #d6d6d6;
    line-height: 50px;
    text-align: center;
}

.w {
    position: relative;
    width: 1200px;
    height: 2000px;
    margin: 0px auto;
    background-color: #f5f5f7;
    padding-left: 100px;
}


el-tag {
    white-space: normal;
    height: auto;
    text-align: center;
}

.left-box {
    position: absolute;
    width: 400px;
    left: 50px;
    top: 100px;
}

.right-box {
    position: absolute;
    width: 600px;
    top: 72px;
    left: 500px;
}

.floor {
    margin-bottom: 8px;
    width: 600px;
    min-height: 50px;
}

.elrow {
    margin-left: 0px !important;
}


.temp {
    white-space: normal;
    word-break: break-all;
    word-wrap: break-word;
    background-color: #ecf5ff;
    color: #409eff;
    border: 1px solid #d9ecff;
    font-size: 10px;
    padding: 3px 8px;
    border-radius: 2px;
    margin-bottom: 6px;
}

</style>
