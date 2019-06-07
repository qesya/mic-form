<template>

    <div class="mic-contain" style="margin-top: 100px">

        <div class="bts-container">
            <div class="mic-CotactFormContainer">
                <h1>Contact Us</h1>
            <template>

                <!-- Form Field Start -->
                <v-form
                        ref="form"
                        v-model="valid"
                        lazy-validation
                        id="chetform">
                    <v-text-field
                            v-model="name"
                            :rules="nameRules"
                            label="Your Full Name"
                            box
                            required
                    ></v-text-field>

                    <v-text-field
                            v-model="business"
                            :rules="businessRules"
                            label="Your Business Name"
                            box
                            required
                    ></v-text-field>

                    <v-text-field
                            v-model="email"
                            :rules="emailRules"
                            label="E-mail"
                            box
                            required
                    ></v-text-field>

                    <v-text-field
                            v-model="telephone"
                            :rules="telephoneRules"
                            label="Your Telephone Number"
                            box
                            required
                    ></v-text-field>

                    <v-textarea
                            name="input-7-1"
                            label="Notes"
                            v-model="notes"
                            :rules="notesRules"
                            box
                    ></v-textarea>

                    <v-text-field
                            v-model="reference"
                            label="Reference"
                            box
                            required
                    ></v-text-field>


                    <v-btn
                            :disabled="!valid"
                            color="success"
                            @click="validate"
                            large
                            class="white--text"
                    >
                        Submit
                    </v-btn>

                    <!-- Form Field End -->


                </v-form>
            </template>
        </div>
            <br clear="all"/>
        </div>

        <!--Success Message Dialog Box Start-->
        <v-dialog
                v-model="dialog"
                width="500"
        >
            <v-card>
                <v-card-title
                        class="headline white--text"
                        primary-title
                        style="background-color:#41bec8"
                >
                    Thank you for using Make It Cheaper
                </v-card-title>

                <v-card-text>
                    We’ve got your request for a Contact us, and one of our team will be in touch soon. We look forward to speaking with you and saving money for your business.
                </v-card-text>

                <v-divider></v-divider>

                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                            color="red"
                            @click="dialog = false"
                            class="white--text"
                    >
                        Close
                    </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
        <!--Success Message Dialog Box End-->

    </div>

</template>

<script>
    import axios from 'axios'
    export default {
        data: () => ({
            valid: true,
            name: '',
            nameRules: [
                v => !!v || 'Name is required',
                v => /\s*[A-Za-z]+(?:\s+[A-Za-z]+)+$/.test(v) || 'Name must contain Firstname and Lastname separated by space (e.g. \'John Smith\')',
                v => (v && v.length <= 100) || 'Name must be less than 100 characters'
            ],
            business: '',
            businessRules: [
                v => !!v || 'Business Name is required',
                v => (v && v.length <= 100) || 'Business Name must be less than 100 characters'
            ],
            email: '',
            emailRules: [
                v => !!v || 'E-mail is required',
                v => /^([a-zA-Z0-9_\-\\.]+)@([a-zA-Z0-9_\-\\.]+)\.([a-zA-Z]{2,5})$/.test(v) || 'E-mail must be valid',
                v => (v && v.length <= 80) || 'Email must be less than 80 characters'
            ],
            telephone: '',
            telephoneRules: [
                v => !!v || 'Telephone Number is required',
                v => (v && (v.trim()).length >= 11 && /^[0][0-9].{9}$/.test(v.trim())) || 'Telephone number needs 11 numbers'
            ],
            contactTime: '',
            notes: '',
            notesRules: [
                v => (v && v.length <= 255) || 'Notes must be less than 255 characters'
            ],
            reference: '',
            dialog: false,

            access_token: '',
            pGUID: '',
            pAccName: '',
            pPartner: '',
            contact_time: ''    /*  those specify for future use */


        }),

        methods: {
            validate () {
                if (this.$refs.form.validate()) {
                    const formData = new FormData(); /*Converting Data into Form Format*/

                    formData.append("pGUID", "82B5510C-6B7F-1234-B8CD-0FA34DD35AD8");           /*Temp Data Need to change when Dynamic */
                    formData.append("pAccName", "testAcountName");                             /*Temp Data Need to change when Dynamic */
                    formData.append("pPartner", "testPartnerName");                              /*Temp Data Need to change when Dynamic */
                    formData.append("name", this.name);
                    formData.append("business_name", this.business);
                    formData.append("telephone_number", this.telephone);
                    formData.append("email", this.email);

                    var moment = require('moment'); /* Get Current Time and Date Using Moment.js */

                    formData.append("contact_time", moment().format('YYYY-MM-DD HH:mm:ss')); /* specify the required formatted time */
                    formData.append("notes", this.notes);
                    formData.append("reference", this.reference);

                    /* Axios Api Call Start */
                    axios({
                        method: 'post',
                        url: 'http://mic-leads.dev-test.makeiteasy.com/api/v1/create?access_token=d86ddc8a9e6127b96572567747a99f07',
                        data: formData,
                        config: { headers: {'Content-Type': 'multipart/form-data;' }}
                    })
                        .then( (response) => {
                            //handle success

                            if(response.status === 201) {
                                this.dialog = true;
                            }
                        })
                        .catch( (error) => {
                            //handle error
                            console.log(error.response);
                        });
                    /* Axios Api Call End */
                }
            },
            reset () {
                this.$refs.form.reset()
            }
        },
        mounted() {

            /*let now = new Date();
            console.log(now.getFullYear()+'-'+now.getMonth()+'-'+now.getDate()+' '+now.getHours()+':'+now.getMinutes()+':'+now.getSeconds())*/
        }
    }
</script>

<style>

    /* Specify Styles Form and Container Start */

    .mic-CotactFormContainer{
        width: 580px;
        background-color: #ffffff;
        padding: 20px 25px 25px 30px;
        float: right;
        margin-right: 10px;
    }
    .mic-contain{
        background-image: linear-gradient(to bottom, #41bec8, #bcf9f1);
        padding: 10px 0px 10px 0px;
    }

    .bts-container{
        max-width: 1190px;
        margin-left: auto;
        margin-right: auto;
    }
    /* Specify Styles Form and Container End */

    /* Apply Media Query For Responsive Start */

    @media (max-width: 1200px) {

        .bts-container {
            max-width: 1140px;
        }
    }
    @media (max-width: 992px) {
        .bts-container {
            max-width: 960px;
        }
    }
    @media (max-width: 768px) {
        .bts-container {
            max-width: 720px;
        }
        .mic-CotactFormContainer{
            width: 100%;
            margin: 0px;
        }
    }
    @media (max-width: 576px) {
        .bts-container {
            max-width: 540px;
        }
    }
    /* Apply Media Query For Responsive End */

</style>