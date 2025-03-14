<template>
    <h3>全局面板</h3>
    <div class="Panel">
        <div class="leftBox">
            <div class="cardPanel">
                <DetailPanel :msg="nodeMsg" />
            </div>
            <div class="cloudpanel">

            </div>
        </div>
        <div class="mainBox">
            <MainCanvas />
        </div>
        <div class="rightBox">
            <div class="auPanel">
                <AuCoopNetwork @send-node="nodeShow" />
            </div>
            <div class="instPanel">
                <InstCoopNetwork />
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

        const nodeShow = (msg) => {
            nodeMsg.value = msg
            console.log("get:", nodeMsg.value);
        }

        return {
            nodeMsg,
            nodeShow,
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