<template>
    <div id="app">
        <div>
            <h2>商品规格：</h2>
            <div v-for="Spec in SpecList">
                规格名
                <el-select v-model="Spec.SpecName" autocomplete filterable allow-create default-first-option
                           placeholder="请选择">
                    <el-option
                            v-for="item in SpecOptions"
                            :key="item.value"
                            :label="item.value"
                            :value="item.value">
                    </el-option>
                </el-select>


                <el-select v-model="Spec.SpecValueList" multiple filterable allow-create value-key="name"
                           placeholder="请录入规格选项">
                </el-select>
                <el-button type="danger" icon="el-icon-delete" circle></el-button>
            </div>
            <el-button @click="addSpec()" type="primary"><i class="el-icon-plus"></i>添加规格项</el-button>
        </div>
        <div>
            <h2>规格明细：</h2>

            <el-table
                    :data="SpecDetailList"
                    style="width: 100%">


                <el-table-column
                        v-for="Spec in SpecList"
                        v-if="Spec.SpecName"
                        :label="Spec.SpecName"
                        :prop="Spec.SpecName"
                >
                </el-table-column>


                <el-table-column
                        prop="address"
                        label="SKU"
                        >
                </el-table-column>
                <el-table-column
                        prop="price"
                        label="价格（元)"
                       >
                </el-table-column>
                <el-table-column
                        prop="name"
                        label="库存"
                        >
                </el-table-column>

            </el-table>


        </div>


    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                label: '商品规格',
                SpecOptions: [{
                    value: '规格',
                }, {
                    value: '颜色',
                }, {
                    value: '款式',
                }, {
                    value: '网络',
                }, {
                    value: '型号',
                }],
                //用户输入数据
                SpecList: [],
                specDetailList:[]

            }
        },

        methods: {
            addSpec() {
                let newSpec = {
                    SpecName: '',
                    SpecValueList: []
                };
                this.SpecList.push(newSpec);
            },
            delSpec() {

//                this.SpecList.push(newSpec);
            }

        },
        computed: {
//            tags() {
//                return this.value10.map(tag => {
//                    if (typeof tag === 'string') {
//                        return {
//                            id: tag,
//                            name: tag.toUpperCase(),
//                            type: 'tag'
//                        }
//                    } else {
//                        return tag
//                    }
//                })
//            }
        }


    }
</script>

<style lang="scss">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .el-tag + .el-tag {
        margin-left: 10px;
    }

    .button-new-tag {
        margin-left: 10px;
        height: 32px;
        line-height: 30px;
        padding-top: 0;
        padding-bottom: 0;
    }

    .input-new-tag {
        width: 90px;
        margin-left: 10px;
        vertical-align: bottom;
    }
</style>
