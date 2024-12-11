<template>
    <header>
        <h1 style="font-size: 16px; color: white;">Group 6 | Ultrasonic Sensor</h1>
    </header>
    <section class="component-section">
        <h1>Distance: {{ displayDistance }}</h1>
        <div class="button-section">
            <div 
                class="distance-button" 
                :class="{ active: distanceUnit === 'cm' }" 
                @click="setUnit('cm')"
            >
                Centimeters
            </div>
            <div 
                class="distance-button" 
                :class="{ active: distanceUnit === 'm' }" 
                @click="setUnit('m')"
            >
                Meters
            </div>
        </div>
    </section>
    <section class="component-section">
        <h1>Rotation: {{ rotationAngle }} Degrees</h1>
        <div class="button-section">
            <div class="rotation-button" @click="fetchRotation('rotate-right')">Rotate Right</div>
            <div class="rotation-button" @click="fetchRotation('loiter')">Loiter</div>
            <div class="rotation-button" @click="fetchRotation('rotate-left')">Rotate Left</div>
        </div>
        <p 
            v-if="rotationResponse" 
            :class="{
                'response-success': rotationStatus === 'ok',
                'response-error': rotationStatus !== 'ok',
            }"
            style="font-weight: 700;"
        >
            {{ rotationResponse }}
        </p>
    </section>
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            rotationResponse: null, // Default text
            rotationStatus: null,   // Status from the API response
            rotationAngle: 95,
            socket: null,
            distance: 0, // Raw distance from the WebSocket
            distanceUnit: 'cm', // Current unit of measurement (cm or m)
        };
    },
    computed: {
        displayDistance() {
            // Converts the distance to meters if the unit is 'm'
            return this.distanceUnit === 'm' 
                ? (this.distance / 100).toFixed(2) + ' m' 
                : this.distance.toFixed(2) + ' cm';
        },
    },
    methods: {
        async fetchRotation(action) {
            try {
                const response = await axios.get(`http://192.168.10.112:4040/${action}`);
                this.rotationResponse = response.data.message; // Update response text
                this.rotationStatus = response.data.status.toLowerCase(); // Update status (e.g., 'ok')
            } catch (error) {
                console.error('Error fetching rotation:', error);
                this.rotationResponse = 'Failed to fetch rotation data.';
                this.rotationStatus = 'error';
            }
        },
        setUnit(unit) {
            this.distanceUnit = unit; // Update the unit of measurement
        },
        initializeWebSocket() {
            this.socket = new WebSocket('ws://192.168.10.112:4040/sensor');

            this.socket.onopen = () => {
                console.log('WebSocket connection established');
            };

            this.socket.onmessage = (event) => {
                try {
                    const data = JSON.parse(event.data);
                    if (data.status === 'Success' && data.distance) {
                        this.distance = data.distance;
                    }
                } catch (error) {
                    console.error('Error parsing WebSocket message:', error);
                }
            };

            this.socket.onclose = () => {
                console.log('WebSocket connection closed');
            };

            this.socket.onerror = (error) => {
                console.error('WebSocket error:', error);
            };
        },
    },
    mounted() {
        this.initializeWebSocket(); // Start the WebSocket connection when the component is mounted
    },
    beforeDestroy() {
        if (this.socket) {
            this.socket.close(); // Clean up the WebSocket connection
        }
    },
};
</script>

<style lang="css" scoped>
header {
    width: 100%;
    height: 96px;
    background-color: #0F5CA8;
    display: flex;
    align-items: center;
    justify-content: center;
}

.component-section {
    width: 100%;
    height: 216px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 32px;
}

.component-section h1 {
    font-size: 16px;
    color: black;
    font-weight: 700;
}

.button-section {
    display: flex;
    flex-direction: row;
    gap: 16px;
}

.distance-button {
    width: 103px;
    background-color: #0F5CA8;
    padding-top: 4px;
    padding-bottom: 4px;
    text-align: center;
    color: white;
    font-weight: 500;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
}
.distance-button:active {
    background-color: black;
    transform: translateY(-3px);
}

.rotation-button {
    width: 104px;
    background-color: #0F5CA8;
    padding-top: 4px;
    padding-bottom: 4px;
    text-align: center;
    color: white;
    font-weight: 500;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s ease, transform 0.2s ease;
}
.rotation-button:active {
    background-color: black;
    transform: translateY(-3px);
}

.response-success {
    color: green;
}
.response-error {
    color: red;
}
</style>