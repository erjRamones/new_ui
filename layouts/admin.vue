<template>
   <div>
        <TransitionRoot as="template" :show="sidebarOpen">
            <Dialog as="div" class="relative z-50 lg:hidden" @close="sidebarOpen = false">
                <TransitionChild as="template" enter="transition-opacity ease-linear duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="transition-opacity ease-linear duration-300" leave-from="opacity-100" leave-to="opacity-0">
                    <div class="fixed inset-0 bg-gray-900/80"/>
                </TransitionChild>
                <div class="fixed inset-0 flex">
                    <TransitionChild as="template" enter="transition ease-in-out duration-300 transform" enter-from="-translate-x-full" enter-to="translate-x-0" leave="transition ease-in-out duration-300 transform" leave-from="translate-x-0" leave-to="-translate-x-full">
                        <DialogPanel class="relative mr-16 flex w-full max-w-xs flex-1">
                            <TransitionChild as="template" enter="ease-in-out duration-300" enter-from="opacity-0" enter-to="opacity-100" leave="ease-in-out duration-300" leave-from="opacity-100" leave-to="opacity-0">
                                <div class="absolute left-full top-0 flex w-16 justify-center pt-5">
                                    <button type="button" class="-m-2.5 p-2.5" @click="sidebarOpen = false">
                                        <span class="sr-only text-xs">Close sidebar</span>
                                        <XMarkIcon class="h-5 w-5 text-white" aria-hidden="true"/>
                                    </button>
                                </div>
                            </TransitionChild>
                            <!-- Sidebar component -->
                            <div class="flex grow flex-col gap-y-3 overflow-y-auto bg-gray-900 px-4 pb-4 ring-1 ring-white/10">
                                <div class="flex h-16 shrink-0 items-center">
                                    <img class="h-6 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=500" alt="Your Company"/>
                                </div>
                                <nav class="flex flex-1 flex-col">
                                    <ul role="list" class="flex flex-1 flex-col gap-y-4">
                                        <li>
                                            <ul role="list" class="-mx-2 space-y-1">
                                                <li v-for="item in navigation" :key="item.name">
                                                    <a v-if="!item.children" :href="item.href" :class="[item.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'group flex gap-x-2 rounded-md p-2 text-xs leading-4 font-semibold']">
                                                        <component :is="item.icon" class="h-4 w-4 shrink-0 text-gray-400" aria-hidden="true"/>
                                                        <span class="text-xs">{{ item.name }}</span>
                                                    </a>
                                                    <Disclosure as="div" v-else v-slot="{ open }">
                                                        <DisclosureButton :class="[item.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'group flex items-center w-full text-left rounded-md p-2 gap-x-2 text-xs leading-4 font-semibold']">
                                                            <component :is="item.icon" class="h-4 w-4 shrink-0 text-gray-400" aria-hidden="true"/>
                                                            <span class="text-xs">{{ item.name }}</span>
                                                            <ChevronRightIcon :class="[open ? 'rotate-90 text-white' : 'text-gray-400 hover:text-white', 'ml-auto h-4 w-4 shrink-0']" aria-hidden="true"/>
                                                        </DisclosureButton>
                                                        <DisclosurePanel as="ul" class="mt-1 px-1">
                                                            <div v-for="subItem in item.children" :key="subItem.name">
                                                                <DisclosureButton as="a" :href="subItem.href" :class="[subItem.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'py-1 pr-2 pl-6 flex gap-x-2 rounded-md text-xs leading-4 font-semibold']">
                                                                    <span class="text-xs">{{ subItem.name }}</span>
                                                                </DisclosureButton>
                                                            </div>
                                                        </DisclosurePanel>
                                                    </Disclosure>
                                                </li>
                                            </ul>
                                        </li>
                                        
                                        <li class="mt-auto">
                                            <a href="/settings" class="group -mx-2 flex gap-x-2 rounded-md p-2 text-xs font-semibold leading-4 text-gray-400 hover:bg-gray-800 hover:text-white">
                                                <Cog6ToothIcon class="h-5 w-5 shrink-0" aria-hidden="true"/>
                                                <span class="text-xs">Settings</span>
                                            </a>
                                        </li>
                                    </ul>
                                </nav>
                            </div>
                        </DialogPanel>
                    </TransitionChild>
                </div>
            </Dialog>
        </TransitionRoot>
    
        <!-- Sidebar for desktop -->
        <div class="hidden lg:fixed lg:inset-y-0 lg:z-50 lg:flex lg:w-48 lg:flex-col">
            <!-- Sidebar component -->
            <div class="flex grow flex-col gap-y-3 overflow-y-auto bg-gray-900 px-4 pb-4">
                <div class="flex h-16 shrink-0 items-center">
                    <!-- <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=500" alt="Your Company"/> -->
                </div>
                <nav class="flex flex-1 flex-col">
                    <ul role="list" class="flex flex-1 flex-col gap-y-4">
                        <li>
                            <ul role="list" class="-mx-2 space-y-1">
                                <li v-for="item in navigation" :key="item.name">
                                    <a v-if="!item.children" @click="navigateTo(item.href)" :class="[item.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'cursor-pointer group flex gap-x-2 rounded-md p-2 text-xs leading-4 font-semibold']">
                                        <component :is="item.icon" class="h-4 w-4 shrink-0 text-gray-400" aria-hidden="true"/>
                                        <span class="text-xs">{{ item.name }}</span>
                                    </a>
                                    <Disclosure as="div" v-else v-slot="{ open }">
                                        <DisclosureButton :class="[item.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'group flex items-center w-full text-left rounded-md p-2 gap-x-2 text-xs leading-4 font-semibold']">
                                            <component :is="item.icon" class="h-4 w-4 shrink-0 text-gray-400" aria-hidden="true"/>
                                            <span class="text-xs">{{ item.name }}</span>
                                            <ChevronRightIcon :class="[open ? 'rotate-90 text-white' : 'text-gray-400 hover:text-white', 'ml-auto h-4 w-4 shrink-0']" aria-hidden="true"/>
                                        </DisclosureButton>
                                        <DisclosurePanel as="ul" class="mt-1 px-1">
                                            <div v-for="subItem in item.children" :key="subItem.name">
                                                <DisclosureButton as="a" :href="subItem.href" 
                                                :class="[subItem.current ? 'bg-gray-800 text-white' : 'text-gray-400 hover:text-white hover:bg-gray-800', 'py-1 pr-2 pl-6 flex gap-x-2 rounded-md text-xs leading-4 font-semibold']">
                                                <component :is="subItem.icon" class="h-4 w-4 shrink-0 text-gray-400" aria-hidden="true"/>
                                                <span class="text-xs">{{ subItem.name }}</span>
                                            </DisclosureButton>
                                        </div>
                                    </DisclosurePanel>
                                </Disclosure>
                                </li>
                            </ul>
                        </li>
                       
                        
                        <li class="mt-auto">
                            <a href="/settings" class="group -mx-2 flex gap-x-2 rounded-md p-2 text-xs font-semibold leading-4 text-gray-400 hover:bg-gray-800 hover:text-white">
                                <Cog6ToothIcon class="h-5 w-5 shrink-0" aria-hidden="true"/>
                                <span class="text-xs">Settings</span>
                            </a>
                        </li>
                    </ul>
                </nav>
            </div>
        </div>
        </div>
        
        <!--Header-->
        <div class="lg:pl-60">
            <div class="sticky top-0 z-40 flex h-10 shrink-0 items-center gap-x-4 border-b border-gray-200 bg-white px-4 shadow-sm sm:gap-x-6 sm:px-6 lg:px-8">
                <button type="button" class="-m-2.5 p-2.5 text-gray-700 lg:hidden" @click="sidebarOpen = true">
                    <span class="sr-only text-xs">Open sidebar</span>
                    <Bars3Icon class="h-6 w-6" aria-hidden="true"/>
                </button>
                
                <!-- Separator -->
                 <div class="h-6 w-1 bg-gray-900/10 lg:hidden" aria-hidden="true"></div>
                 <div class="flex flex-1 justify-end gap-x-2.5 self-stretch xs:gap-x-3">
                    <div class="flex items-center  xs:gap-x-3">
                        <button type="button" class="mr-2 p-2 text-gray-400 hover:text-gray-500">
                            <span class="sr-only text-xs">View notifications</span>
                            <BellIcon class="h-5 w-5" aria-hidden="true" />
                        </button>
                        
                        <!-- Separator -->
                         <div class="hidden xs:block xs:h-5 xs:w-0.5 xs:bg-gray-900/10" aria-hidden="true"></div>

                         <!-- Profile dropdown -->
                           <Menu as="div" class="relative">
            <MenuButton class="flex items-center p-1">
                <span class="sr-only text-xs">Open user menu</span>
                <img class="h-6 w-6 rounded-full bg-gray-50"
                    src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
                    alt="" />
                <span class="hidden xs:flex xs:items-center">
                    <span class="ml-2 text-xs font-semibold leading-4 text-gray-900">Admin</span>
                    <ChevronDownIcon class="ml-1 h-4 w-4 text-gray-400" aria-hidden="true" />
                </span>
            </MenuButton>
            <transition enter-active-class="transition ease-out duration-100"
                enter-from-class="transform opacity-0 scale-95" enter-to-class="transform opacity-100 scale-100"
                leave-active-class="transition ease-in duration-75"
                leave-from-class="transform opacity-100 scale-100" leave-to-class="transform opacity-0 scale-95">
                <MenuItems
                    class="absolute right-0 z-10 mt-1.5 w-28 origin-top-right rounded-md bg-white py-1.5 shadow-lg ring-1 ring-gray-900/5 focus:outline-none">
                    <MenuItem v-for="item in userNavigation" :key="item.name" v-slot="{ active }">
                        <a :href="item.href"
                            :class="[active ? 'bg-gray-50' : '', 'block px-2 py-1 text-xs leading-4 text-gray-900']">{{ item.name }}</a>
                    </MenuItem>
                </MenuItems>
            </transition>
        </Menu>
     </div>
        </div>

            </div> 
            
            <main class="py-5">
                <div class="px-4 sm:px-6 ">
                    <!-- Your content -->
                    <keep-alive>
                        <router-view />
                    </keep-alive>
                    <slot />
                </div>
            </main>
        </div>

</template>

<script setup>
import { ref } from "vue";
import {
    Dialog,
    DialogPanel,
    Disclosure,
    DisclosureButton,
    DisclosurePanel,
    Menu,
    MenuButton,
    MenuItem,
    MenuItems,
    TransitionChild,
    TransitionRoot,
} from "@headlessui/vue";
import {
    Bars3Icon,
    BellIcon,
    ChartBarSquareIcon,
    ChartPieIcon,
    Cog6ToothIcon,
    CurrencyDollarIcon,
    DocumentDuplicateIcon,
    FolderIcon,
    HomeModernIcon,
    TruckIcon,
    UsersIcon,
    XMarkIcon,
    LockClosedIcon,
    AdjustmentsHorizontalIcon,
    CreditCardIcon,
    DocumentTextIcon,
    ArrowLongLeftIcon,

} from "@heroicons/vue/24/outline";
import { ChevronRightIcon } from "@heroicons/vue/20/solid";
import {
    ChevronDownIcon,
    ShoppingBagIcon,
} from "@heroicons/vue/24/outline";
import { UserIcon, UserCircleIcon } from "@heroicons/vue/24/outline";

const sidebarOpen = ref(false);
const openPeopleDropdown = ref(false); 


const navigation = [
    { name: "Dashboard", href: "#", icon: ChartPieIcon, current: false },

    { name: "Client Groups", href:"#", icon: ShoppingBagIcon, current: false,},
    
    { name: "Personality", icon: UsersIcon, current: false,
        children: [
            { name: "Employee", href: "/#", icon: TruckIcon, current: false },
            { name: "Admin", href: "/#", icon: UserIcon, current: false },
        ],
    },

    { name: "Transactions", icon: UsersIcon, current: false,
        children: [
            { name: "Loan Application", href: "/#", icon: CurrencyDollarIcon, current: false },
            { name: "Loan Release", href: "/#", icon:  CreditCardIcon, current: false },
            { name: "Payment Schedule", href: "/#", icon: CreditCardIcon, current: false },
        ],
    },

    { name: "Payments", href: "/#", icon: CreditCardIcon, current: false },
    { name: "Report", href: "/#", icon: LockClosedIcon, current: false },
    { name: "Roles / Permissions", href: "/roles", icon: LockClosedIcon, current: false },
    { name: "Help", href: "/#", icon: XMarkIcon , current: false },

];

const userNavigation = [
    { name: "Your profile", href: "/profile" },
    { name: "Sign out", href: "/" },
];

function togglePeopleDropdown() {
    openPeopleDropdown.value = !openPeopleDropdown.value;
}

function closePeopleDropdown() {
    openPeopleDropdown.value = false;
}

function navigateTo(href) {
    window.location.href = href;
    closePeopleDropdown(); 
}
</script>