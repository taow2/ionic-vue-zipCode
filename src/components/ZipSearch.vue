<template>
    <ion-grid>
        <form @submit="onSubmit">
            <ion-col>
                <ion-item>
                    <!-- :value binds to data() below -->
                    <ion-input label="Zipcode: " :value="zip" @input="zip = $event.target.value" placeholder="Enter US Zipcode" name="zip"></ion-input>
                </ion-item>
            </ion-col>
            <ion-col>
                <ion-button type="submit" color="primary" expand="block">Find</ion-button>
            </ion-col>
        </form>
    </ion-grid>
</template>

<script setup>
import { alertController, IonGrid, IonButton, IonCol, IonItem, IonInput} from '@ionic/vue';
</script>

<script>
export default {
    name: "ZipSearch",
    data() {
        return {
            zip: ""
        };
    },
    methods: {
        onSubmit(e) {
            e.preventDefault();

            // check if the input zip is received
            // console.log(this.zip);

            // zip regex
            const isValidZip = /(^\d{5}$)|(^\d{5}-\d{4}$)/.test(this.zip);
            // test for valid zip
            if(!isValidZip) {                
                this.showAlert();
                this.zip="";
            }
            else {
                //  get-zip naming is arbitrary
                this.$emit("get-zip", this.zip);
                this.zip="";
            }
        },
        async showAlert() {
            return await alertController
                .create({
                    header: "Enter Zipcode",
                    message: "Please enter a valid US zipcode",
                    buttons: ["OK"]
                })
                .then(a => a.present());
        }
    }
};
</script>

<script></script>

