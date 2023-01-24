const express = require('express');
const app = express();
const bodyParser = require('body-parser');
const Gumroad = require('gumroad');
const Klaviyo = require('klaviyo');

//Gumroad API
// Change redirect in Gumroad as well as this app

const gumroad = new Gumroad({
    client_id: 'brYzIqvfEGO9rJgvvFErxrAa6WX0jmhBG7GtYbYWrVU',
    client_secret: 'fEFjETDWKFeEX3oIiNwp12Dc4AA_DZAcTzO2n5ZhvTw',
    redirect_uri: 'http://example.com/callback'
});

//Klaviyo API

const klaviyo = new Klaviyo({
    api_key: 'pk_c4ae1c1329d0c887c67522bfa09ef746f8'
});

//Authentication

app.use(bodyParser.json());

app.post('/auth', (req, res) => {
    gumroad.authenticate(function(err, response) {
        if (err) {
            //handle the error
            return res.status(500).send(err);
        } else {
            //store the token
            let token = response.token;

            //get the customer data from Gumroad
            gumroad.get('/customers', {}, function(err, response) {
                if (err) {
                    //handle the error
                    return res.status(500).send(err);
                } else {
                    //store the customer data
                    let customers = response.customers;
                    
                    //loop through the customers
                    customers.forEach(function(customer) {
                        //create the customer in Klaviyo
                        klaviyo.create('/customers', customer, function(err, response) {
                            if (err) {
                                //handle the error
                                return res.status(500).send(err);
                            } else {
                                //customer is successfully created in Klaviyo
                                return res.status(200).send('Successfully created customer in Klaviyo');
                            }
                        });
                    });
                }
            });
        }
    });
});
