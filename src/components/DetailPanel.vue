<template>
    <div class="nodeCard">
        <el-container>
            <el-header class="header">DetailPanel</el-header>
            <el-main class="elmain">
                <table style="width: 100%">
                    <tbody>
                        <tr v-for="(v, k) in nodeMsg">
                            <td class="detail-info">{{ k }}</td>
                            <td class="detail-info">{{ v }}</td>
                        </tr>
                    </tbody>
                </table>
            </el-main>
        </el-container>
    </div>

</template>
<script>
import { ref, watch } from 'vue'
export default {
    props: {
        msg: Object
    },
    setup(props) {
        const nodeMsg = ref(props.msg)

        watch(
            () => props.msg,
            (newValue) => {
                nodeMsg.value = newValue;
                console.log("show:", nodeMsg.value);

            },
            { immediate: true }
        );

        return {
            nodeMsg
        }
    }

}
</script>

<style scoped>
.nodeCard {
    height: 50vh;
    width: 100%;
    border: 2px solid #000;
    text-align: center;
    font-size: 18px;
}

.header {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 10vh;
    background-color: rgba(128, 128, 128, 0.6);
}

.elmain {
    text-align: left;
    line-height: 30px;
    height: 40vh;
}
</style>