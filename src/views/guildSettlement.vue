<template>
    <div class="guild-page">
        <el-steps :active="active" align-center>
            <el-step title="公会信息" />
            <el-step title="营业执照" />
            <el-step title="负责人信息" />
        </el-steps>
        <div class="form-one">
             <el-form v-if="active === 1" label-width="120px" ref="ruleForm1" :model="formState1" :rules="rulesOne">
                <el-form-item label="机构名称" prop="company">
                    <el-input v-model="formState1.company" style="width: 200px" placeholder="最多20字" maxlength="20"/>
                </el-form-item>
                <el-form-item label="入驻平台" prop="platform">
                <el-select v-model="formState1.platform" placeholder="请选择">
                    <el-option label="抖音" value="0" />
                    <el-option label="西瓜" value="1" />
                    <el-option label="火山" value="2" />
                </el-select>
                </el-form-item>
                <el-form-item label="经营能力">
                    <el-checkbox-group v-model="formState1.ability">
                        <el-checkbox label="主播培训" name="0" />
                        <el-checkbox label="线下场地" name="1" />
                        <el-checkbox label="短视频拍摄" name="2" />
                    </el-checkbox-group>
                </el-form-item>
                <el-form-item label="机构介绍">
                    <el-input v-model="formState1.profile" type="textarea" maxlength="200" />
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit(ruleForm1)">下一步</el-button>
                    <el-button>上一步</el-button>
                </el-form-item>
            </el-form>
             <el-form v-if="active === 2" :model="formState2" label-width="120px" ref="ruleForm2" :rules="rulesTwo">
                <el-form-item label="营业执照" prop="photo">
                      <el-upload
                        class="avatar-uploader"
                        action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
                        :show-file-list="false"
                        :on-success="handleAvatarSuccess"
                    >
                        <img v-if="formState2.photo" :src="formState2.photo" class="avatar" />
                        <el-icon v-else class="avatar-uploader-icon"><Plus /></el-icon>
                    </el-upload>
                </el-form-item>
                <el-form-item label="证件号" prop="cardNumber">
                    <el-input v-model="formState2.cardNumber" placeholder="15位或18位" />
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit(ruleForm2)">下一步</el-button>
                    <el-button>上一步</el-button>
                </el-form-item>
            </el-form>
             <el-form v-if="active === 3" :model="formState3" label-width="120px" ref="ruleForm3" :rules="rulesThree">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="formState3.name" placeholder="最多10个字" />
                </el-form-item>
                <el-form-item label="身份证" prop="id">
                    <el-input v-model="formState3.id" placeholder="11位字符" maxlength="10"/>
                </el-form-item>
                <el-form-item label="联系方式" prop="phone">
                    <el-input v-model="formState3.phone" />
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit(ruleForm3)">提交</el-button>
                    <el-button>上一步</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>


<script>
import { reactive, toRefs } from '@vue/reactivity'
export default {
    name: 'GuildSettlement',
    setup() {
        const state = reactive({
            formState1: {
                company: '',
                platform: '',
                ability: [],
                profile: '',
            },
            formState2 : {
                photo: '',
                cardNumber: '',
            },
            formState3: {
                name: '',
                id: '',
                phone: '',
            },
            ruleForm1: null,
            ruleForm2: null,
            ruleForm3: null,
            active: 1,
            rulesOne: {
                company: [
                    { required: true, message: '请输入机构名称', tigger: 'blur'}
                ],
                platform: [
                    { required: true, message: '请选择入驻平台', tigger: 'blur'}
                ]
            },
            rulesTwo: {
                photo: [
                    { required: true, message: '营业执照未上传', tigger: 'blur'}
                ],
                cardNumber: [
                    { required: true, message: '证件号码未填写', tigger: 'blur'}
                ]
            },
            rulesThree: {
                name: [
                    { required: true, message: '姓名未填写', tigger: 'blur'}
                ],
                id: [
                    { required: true, message: '身份证号未填写', tigger: 'blur'}
                ],
                phone: [
                    { required: true, message: '联系方式未填写', tigger: 'blur'}
                ],
            }
        })

        const onSubmit= () => {
            state[`ruleForm${state.active}`].validate((valid, obj) => {
                console.log(valid, obj, '1')
                if (valid) {
                    if ([1, 2].includes(state.active)) {
                        state.active = state.active + 1
                    } else {
                        console.log('提交成功')
                    }
                } else {
                    if (!obj) return
                    if (obj.platform) {
                        console.log('入驻平台未选择')
                    } else if(obj.company){
                        console.log('机构名称未填写')
                    } else if (obj.photo) {
                        console.log('营业执照未上传')
                    } else if (obj.cardNumber) {
                        console.log('营业执照未上传')
                    } else if (obj.photo) {
                        console.log('证件号码未填写')
                    }else if(obj.name) {
                        console.log('姓名未填写')
                    } else if(obj.id){
                        console.log('机构名称未填写')
                    } else if (obj.phone) {
                        console.log('营业执照未上传')
                    }
                    return false
                }
            })
        }

        return {
            ...toRefs(state),
            onSubmit,
        }
    },
}
</script>

<style lang="less" scoped>
.guild-page {
    .form-one {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
    }

    .el-steps {
        height: 200px !important;
        border-bottom: 1px solid #999;

        .el-step {
            margin-top: 50px;
        }
    }

    .avatar-uploader .avatar {
        width: 178px;
        height: 178px;
        display: block;
    }

    .avatar-uploader .el-upload {
        border: 1px dashed var(--el-border-color);
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
        transition: var(--el-transition-duration-fast);
        }

        .avatar-uploader .el-upload:hover {
        border-color: var(--el-color-primary);
        }

        .el-icon.avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        text-align: center;
    }
}
</style>