<template>
    <!-- <h3>全局面板</h3> -->
    <div class="Panel">
        <div class="leftBox">
            <div class="cardPanel">
                <DetailPanel :msg="nodeMsg" @sendNet="netShow" />
            </div>
            <div class="cloudpanel">

            </div>
        </div>
        <div class="mainBox">
            <MainCanvas @send-node="nodeShow" />
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


import { ref } from 'vue'
export default {
    components: {
        MainCanvas,
        DetailPanel,
        AuCoopNetwork,
        InstCoopNetwork,
    },
    setup() {
        const nodeMsg = ref(null);
        const auNetInfo = ref(null);
        const instNetInfo = ref(null);

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

        return {
            nodeMsg,
            nodeShow,
            netShow,
            auNetInfo,
            instNetInfo
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
</style>