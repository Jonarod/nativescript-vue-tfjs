<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
        <FlexboxLayout flexDirection="column" justifyContent="center" alignItems="center">
            <Label class="message" :text="msg" col="0" row="0"/>
            <Button text="Tensorflow.js works !" @tap="works"></Button>
            <Button text="Well... not really" @tap="notWorking"></Button>
        </FlexboxLayout>
    </Page>
</template>

<script>
    import * as tf from '@tensorflow/tfjs';

    export default {
        data() {
            return {
                msg: 'Check results in console'
            }
        },
        methods: {
            works(){
                tf.scalar(3.14).print()
            },
            notWorking(){
                // Define a model for linear regression.
                const model = tf.sequential();

                model.add(tf.layers.dense({units: 1, inputShape: [1]}))

                // Prepare the model for training: Specify the loss and the optimizer.
                model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});
                
                // Generate some synthetic data for training.
                const xs = tf.tensor2d([1, 2, 3, 4], [4, 1]);
                const ys = tf.tensor2d([1, 3, 5, 7], [4, 1]);
                
                // Train the model using the data.
                model.fit(xs, ys, {epochs: 10}).then(() => {
                    // Use the model to do inference on a data point the model hasn't seen before:
                    model.predict(tf.tensor2d([5], [1, 1])).print();
                }).catch(e => {console.log(e)}) // throws error [TypeError: process.hrtime is not a function]

            }
        }
    }
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
