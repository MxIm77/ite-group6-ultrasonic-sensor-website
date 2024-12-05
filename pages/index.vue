<template>
    <header>
        <h1 style="font-size: 16px; color: white;">Group 6 | Ultrasonic Sensor</h1>
    </header>
    <section class="component-section">
        <h1>Distance: 15.5 cm</h1>
        <div class="button-section">
            <div class="distance-button">Centimeters</div>
            <div class="distance-button">Meters</div>
        </div>
    </section>
    <section class="component-section">
        <h1>Rotation: 90 Degrees</h1>
        <div class="button-section">
            <div class="rotation-button">Rotate Right</div>
            <div class="rotation-button">Loiter</div>
            <div class="rotation-button">Rotate Left</div>
        </div>
    </section>
</template>

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
}
</style>