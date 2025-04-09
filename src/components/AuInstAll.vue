<template>
    <div class="auPanel">
        <AuCoopNetwork @send-node="nodeShowMid" :clusterMsg="auClusterMsg" />
    </div>
    <div class="instPanel">
        <InstCoopNetwork @send-node="nodeShowMid" :clusterMsg="instClusterMsg" />
    </div>
</template>

<script>
import AuCoopNetwork from './AuCoopNetwork.vue'
import InstCoopNetwork from './InstCoopNetwork.vue'
import { ref, watch } from 'vue'

export default {
    components: {
        AuCoopNetwork,
        InstCoopNetwork,
    },
    props: {
        auClusterMsg: Number,
        instClusterMsg: Number,
        topicMsg: Number,
    },
    emits: ["nodeShowToTop"],
    setup(props, { emit }) {
        const nodeShowMid = (msg) => {
            emit("nodeShowToTop", msg);
        }
        return {
            nodeShowMid
        }
    }
}
</script>