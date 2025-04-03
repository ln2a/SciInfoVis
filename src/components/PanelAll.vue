<template>
    <!-- <h3>全局面板</h3> -->
    <div class="Panel">
        <div class="leftBox">
            <div class="cardPanel">
                <DetailPanel :msg="nodeMsg" @sendNet="netShow" @send-topic="topicShow" @back-panel="backPanel" />
            </div>
            <div class="cloudpanel">
                <KeyWordsCloud :msg="nodeMsg" />
            </div>
        </div>
        <div class="mainBox">
            <KeepAlive>
                <component :is="currentComponent" @send-node="nodeShow" :topicMsg="topicInfo" />
            </KeepAlive>
            <!-- <MainCanvas @send-node="nodeShow" /> -->
            <!-- <PaperCoopNetwork /> -->
        </div>
        <div class="rightBox">
            <div class="auPanel">
                <AuCoopNetwork @send-node="nodeShow" :clusterMsg="auNetInfo" />
            </div>
            <div class="instPanel">
                <InstCoopNetwork @send-node="nodeShow" :clusterMsg="instNetInfo" />
            </div>
        </div>
    </div>
</template>

<script>
import MainCanvas from './MainCanvas.vue';
import AuCoopNetwork from './AuCoopNetwork.vue'
import InstCoopNetwork from './InstCoopNetwork.vue'
import DetailPanel from './DetailPanel.vue'
import PaperCoopNetwork from "./PaperCoopNetwork.vue";
import KeyWordsCloud from "./KeyWordsCloud.vue";

import { ref } from 'vue'
export default {
    components: {
        MainCanvas,
        DetailPanel,
        AuCoopNetwork,
        InstCoopNetwork,
        PaperCoopNetwork,
        KeyWordsCloud,
    },
    setup() {
        const nodeMsg = ref(null);
        const topicInfo = ref(null);
        const auNetInfo = ref(null);
        const instNetInfo = ref(null);
        const currentComponent = ref("MainCanvas")

        const nodeShow = (msg) => {
            nodeMsg.value = msg
            console.log("get:", nodeMsg.value);
        }

        const netShow = (group, cluster) => {
            console.log(group);
            console.log(cluster);
            if (group == 2)
                auNetInfo.value = cluster
            if (group == 3)
                instNetInfo.value = cluster
            console.log(instNetInfo.value);
            console.log(auNetInfo.value);
            // console.log(typeof (auNetInfo.value));
        }

        const topicShow = (cluster) => {
            // 切换组件
            console.log("get", cluster);
            currentComponent.value = (currentComponent.value != "MainCanvas") ?
                "MainCanvas" : "PaperCoopNetwork";
            topicInfo.value = cluster;
        }

        const backPanel = () => {
            currentComponent.value = (currentComponent.value != "MainCanvas") ?
                "MainCanvas" : "PaperCoopNetwork";
        }

        return {
            nodeMsg,
            nodeShow,
            netShow,
            topicShow,
            auNetInfo,
            instNetInfo,
            currentComponent,
            topicInfo,
            backPanel
        }
    }
}
</script>

<style scoped>
.Panel {
    display: flex;
    border: 1px solid #000;
}

.leftBox {
    width: 20%;
    height: 100vh;
    border: 1px solid pink;
}

.mainBox {
    width: 50%;
    height: 100vh;
    border: 1px solid pink;
}

.rightBox {
    width: 30%;
    height: 100vh;
    border: 1px solid pink;
}

.Panel {
    overflow: hidden;
}
</style>