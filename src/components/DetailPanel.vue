<template>
    <div class="nodeCard">
        <el-container>
            <el-header class="header">DetailPanel</el-header>
            <div class="demain" v-show="cardFlag">
                <table>
                    <tbody>
                        <tr v-if="typeof nodeMsg === 'object'" v-for="(v, k) in nodeMsg">
                            <td>{{ k }}</td>
                            <td>{{ v }}</td>
                        </tr>
                        <tr v-else>
                            <td style="width: 300px; font-size: 16px; white-space: pre-wrap;">{{ nodeMsg }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
            <div class="btnController">
                <div class="mb-4">
                    <el-button v-show="networkFlag" @click="netStart" type="primary">NetWork</el-button>
                    <el-button v-show="topicFlag" @click="topicStart" type="success">Topic Info</el-button>
                    <el-button v-show="backFlag" @click="backToPanel" type="warning">Back</el-button>
                </div>
            </div>
            <!-- <div class="link">
                <p style="white-space: pre-wrap;">{{ linkData[1][0] }}</p>
            </div> -->
        </el-container>
    </div>

</template>
<script>
import linkInfo from '../../../output_data/topicInfoJson.json'
import { ref, watch } from 'vue'
export default {
    props: {
        msg: Object
    },
    emits: ["sendNet", "sendTopic", "backPanel"],
    setup(props, { emit }) {
        const nodeMsg = ref(props.msg)
        const networkFlag = ref(false)
        const topicFlag = ref(false)
        const cardFlag = ref(false)
        const backFlag = ref(false)
        const linkData = ref(linkInfo)

        watch(
            () => props.msg,
            (newValue) => {
                if (newValue !== null) {
                    cardFlag.value = true
                    if (newValue.id !== "") {
                        nodeMsg.value = newValue;
                        if (newValue.group === 1) {
                            if (backFlag.value != true) {
                                if (newValue.cluster !== 0)
                                    topicFlag.value = true
                                else
                                    topicFlag.value = false
                                networkFlag.value = false
                            }
                        }
                        else if (newValue.group === 2 || newValue.group === 3) {
                            topicFlag.value = false
                            networkFlag.value = true
                        }
                        else {
                            // console.log("是link");
                            nodeMsg.value = linkData.value[newValue.cluster][newValue.topic]
                            // console.log(linkData.value[newValue.cluster][newValue.topic]);
                        }
                    }

                    // delete nodeMsg.value.group;
                    console.log("show:", nodeMsg.value);

                    // console.log(topicFlag.value);
                }
            },
            { immediate: true }
        );

        function netStart() {
            console.log(nodeMsg.value.cluster);
            emit("sendNet", nodeMsg.value.group, nodeMsg.value.cluster)
        }

        function topicStart() {
            console.log(nodeMsg.value.cluster);

            emit("sendTopic", nodeMsg.value.cluster)
            topicFlag.value = false
            backFlag.value = true
        }

        function backToPanel() {
            topicFlag.value = true
            backFlag.value = false
            emit("backPanel");
        }

        return {
            nodeMsg,
            networkFlag,
            topicFlag,
            netStart,
            cardFlag,
            topicStart,
            backFlag,
            backToPanel,
            linkData
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
    /* line-height: 30px; */
    height: 40vh;

}

.demain {
    width: 100%;
    height: 30vh;
    overflow-y: scroll;
    border-bottom: 3px solid #aaa;
}

td {
    font-size: 12px;
    /* max-width: 80%; */
    text-align: left;
    border-bottom: 1px solid #aaa;
    width: 150px;
    /* 设定固定宽度 */
    word-break: break-word;
    /* 防止超出时不换行 */
}

.btnController {
    height: 10vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>