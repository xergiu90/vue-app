<template>
    <div class="container-fluid">
        <div class="row h-100">
            <div class="col-md-4 border-right border-dark h-100">
                <div class="filter-group">
                    <input class="mt-3 w-100" id="filter" aria-describedby="filter" placeholder="filter..."  v-model="filter" v-on:input="filterContacts">

                    <div class="advanced-fitler-group">
                        <button type="button" class="btn btn-link" v-on:click="clearFilter">clear filter</button>
                        <button type="button" class="btn btn-link" data-toggle="modal" data-target="#modalFilterCenter">advanced filter</button>
                    </div>


                    <!-- Modal -->
                    <div class="modal fade" id="modalFilterCenter" tabindex="-1" role="dialog" aria-labelledby="modalFilterTitle" aria-hidden="true">
                        <div class="modal-dialog modal-dialog-centered" role="document">
                            <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="modalFilterTitle">Advanced filter</h5>
                                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                        <span aria-hidden="true">&times;</span>
                                    </button>
                                </div>
                                <div class="modal-body m-5">
                                    <div class="form-group row">
                                        <label for="name">Name filter</label>
                                        <input class="form-control" id="name" aria-describedby="nameHelp" placeholder="Enter last name" required  v-model="nameFilter">
                                        <div class="invalid-feedback">
                                            Last name is required!
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="groupFilter">Group</label>
                                        <select id="groupFilter" class="form-control" v-model="groupFilter">
                                            <option selected value="work">Work</option>
                                            <option value="friends">Friends</option>
                                            <option value="family">Family</option>
                                            <option value="others">Others</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
                                    <button type="button" class="btn btn-primary"  v-on:click="advancedFilterContacts" data-dismiss="modal">OK</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="contact-list">
                    <div class="mt-3" v-for="(value, name) in groupedContacts" v-bind:key="name">
                        <span class="group-letter">{{ name }}</span>
                        <ul class="contact-list">
                            <li class="contact" v-for="contact in value" v-bind:key="contact.firstName" v-on:click="showDetails(contact)">{{contact.firstName + ' ' + contact.lastName.toUpperCase()}}</li>
                        </ul>
                    </div>
                </div>

                <button type="button" class="add-contacts btn btn-outline-secondary" data-toggle="modal" data-target="#modalCenter">Add new contact</button>

                <!-- Modal -->
                <div class="modal fade" id="modalCenter" tabindex="-1" role="dialog" aria-labelledby="modalCenterTitle" aria-hidden="true">
                    <div class="modal-dialog modal-dialog-centered" role="document">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h5 class="modal-title" id="modalLongTitle">Add Contact</h5>
                                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                    <span aria-hidden="true">&times;</span>
                                </button>
                            </div>
                            <div class="modal-body m-5">
                                <div class="form-group row">
                                    <label for="firstname">First name</label>
                                    <input class="form-control" id="firstname" aria-describedby="firstnameHelp" placeholder="Enter first name" required  v-model="firstName">
                                    <div class="invalid-feedback">
                                        First name is required!
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <label for="lastname">First name</label>
                                    <input class="form-control" id="lastname" aria-describedby="lastnameHelp" placeholder="Enter last name" required  v-model="lastName">
                                    <div class="invalid-feedback">
                                        Last name is required!
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <label for="group">Group</label>
                                    <select id="group" class="form-control" v-model="group">
                                        <option selected value="work">Work</option>
                                        <option value="friends">Friends</option>
                                        <option value="family">Family</option>
                                        <option value="others">Others</option>
                                    </select>
                                </div>
                            </div>
                            <div class="modal-footer">
                                <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
                                <button type="button" class="btn btn-primary"  v-on:click="addContact" data-dismiss="modal">OK</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-md-8">
                <div class="contact-details h-75">
                    <h1>{{currentUser.firstName + ' ' + currentUser.lastName}}</h1>
                    <div>{{currentUser.group}}</div>
                </div>
                <div class="action-buttons">
                    <button type="button" class="btn btn-outline-secondary" data-toggle="modal" data-target="#modalEditCenter">Edit contact</button>
                    <button type="button" class="btn btn-outline-secondary" v-on:click="deleteContact(currentUser)">Delete contact</button>

                    <!-- Modal -->
                    <div class="modal fade" id="modalEditCenter" tabindex="-1" role="dialog" aria-labelledby="modalCenterTitle" aria-hidden="true">
                        <div class="modal-dialog modal-dialog-centered" role="document">
                            <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="modalEditLongTitle">Edit Contact</h5>
                                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                        <span aria-hidden="true">&times;</span>
                                    </button>
                                </div>
                                <div class="modal-body m-5">
                                    <div class="form-group row">
                                        <label for="firstnameEdit">First name</label>
                                        <input class="form-control" id="firstnameEdit" aria-describedby="firstnameHelp" placeholder="Enter first name" required  v-model="temporaryCurrentUser.firstName">
                                        <div class="invalid-feedback">
                                            First name is required!
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="lastnameEdit">First name</label>
                                        <input class="form-control" id="lastnameEdit" aria-describedby="lastnameHelp" placeholder="Enter last name" required  v-model="temporaryCurrentUser.lastName">
                                        <div class="invalid-feedback">
                                            Last name is required!
                                        </div>
                                    </div>
                                    <div class="form-group row">
                                        <label for="groupEdit">Group</label>
                                        <select id="groupEdit" class="form-control" v-model="temporaryCurrentUser.group">
                                            <option selected value="work">Work</option>
                                            <option value="friends">Friends</option>
                                            <option value="family">Family</option>
                                            <option value="others">Others</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancel</button>
                                    <button type="button" class="btn btn-primary"  v-on:click="editContact(currentUser)" data-dismiss="modal">OK</button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Home',
    data() {
        let contacts = [
            {
                "firstName" : "al" ,
                "lastName" : "pacino" ,
                "group" : "friends"
            },
            {
                "firstName": "alain" ,
                "lastName" : "delon" ,
                "group" : "friends"
            },
            {
                "firstName" : "anthony" ,
                "lastName" : "hopkins" ,
                "group" : "work"
            },
            {
                "firstName" : "brad" ,
                "lastName" : "pitt" ,
                "group" : "friends"
            },
            {
                "firstName" : "joaquin" ,
                "lastName" : "phoenix" ,
                "group" : "work"
            },
            {
                "firstName" : "matt" ,
                "lastName" : "damon" ,
                "group" : "friends"
            },
            {
                "firstName" : "michael" ,
                "lastName" : "caine" ,
                "group" : "others"
            },
            {
                "firstName" : "morgan" ,
                "lastName" : "freeman" ,
                "group" : "work"
            },
            {
                "firstName" : "tommy" ,
                "lastName" : "lee jones" ,
                "group" : "family"
            }
        ];


       return {
            currentUser: {"firstName": "Jhon", "lastName": "Doe", "group": "friends"},
            temporaryCurrentUser: {"firstName": "Jhon", "lastName": "Doe", "group": "friends"},
            password: null,
            filter:null,
            lastName:null,
            firstName:null,
            nameFilter:'',
            groupFilter:"work",
            group:"work",
            groupedContacts: this.getContacts(contacts),
            contacts: contacts,
            temporaryContacts: Object.assign([], contacts)
       }
    },
    methods: {
        getContacts(contacts) {
            const groupContacts={};

            contacts.sort((a,b) => (a.firstName > b.firstName) ? 1 : ((b.firstName > a.firstName) ? -1 : 0));
            contacts.forEach(
                (el) => {
                    if (groupContacts[el.firstName.charAt(0)]){
                        groupContacts[el.firstName.charAt(0)].push(el);
                    } else {
                        groupContacts[el.firstName.charAt(0)] = [el];
                    }
                }
            );

            return groupContacts;
        },
        addContact() {
            let newContact = {"firstName": this.firstName, "lastName": this.lastName, "group": this.group};

            this.contacts.push(newContact);
            this.groupedContacts = this.getContacts(this.contacts);
            this.lastName = null;
            this.firstName = null;
            this.group = null;
        },
        editContact(modifiedContact) {
            const index = this.contacts.findIndex(contact => contact.firstName === modifiedContact.firstName);

            this.contacts[index].firstName= this.temporaryCurrentUser.firstName;
            this.contacts[index].lastName= this.temporaryCurrentUser.lastName;
            this.contacts[index].group= this.temporaryCurrentUser.group;
            this.currentUser = Object.assign({}, this.temporaryCurrentUser);

        },
        deleteContact(deletedContact) {
            const index = this.contacts.findIndex(contact => contact.firstName === deletedContact.firstName);

            delete this.contacts[index];
            this.groupedContacts = this.getContacts(this.contacts);
            this.currentUser = {"firstName": "", "lastName": "", "group": ""};
        },
        showDetails(currentUser) {
            this.currentUser = Object.assign({}, currentUser);
            this.temporaryCurrentUser = Object.assign({}, currentUser);
        },
        filterContacts() {
            if(this.filter === ''){
                this.groupedContacts = this.getContacts(this.contacts);
            } else {
                this.groupedContacts = this.getContacts(this.contacts.filter(contact => (contact.firstName.toLowerCase().includes(this.filter.toLowerCase()) || contact.lastName.toLowerCase().includes(this.filter.toLowerCase()))));
            }
        },
        advancedFilterContacts() {
            this.groupedContacts = this.getContacts(this.contacts.filter(contact => ((contact.firstName.toLowerCase().includes(this.nameFilter.toLowerCase()) || contact.lastName.toLowerCase().includes(this.nameFilter.toLowerCase())) && contact.group === this.groupFilter)));
        },
        clearFilter(){
            this.groupedContacts = this.getContacts(this.contacts);
        }
    }

}




</script>


