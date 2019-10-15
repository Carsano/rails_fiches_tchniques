
#MobileContract

## Model MobilContract

has_many :join_table_mobil_contracts
has_many :mobil_simulations, through: :join_table_mobil_contracts

rails g model MobilContract supplier:string offer_name:string lign_service_price:integer sim_card_price:integer engagement:boolean add_phone:boolean bundle_price:float bundle_gbyte:float calls_france:boolean  calls_europe:boolean gbyte_europe:float calls_internationnal:boolean net_internationnal:boolean

## migration

supplier:string
offer_name:string
lign_service_price:integer
sim_card_price:integer
engagement:boolean, default:false
add_phone:boolean
bundle_price:float, default: 0.00
bundle_gbyte:float, default: 0.00
calls_france:boolean
calls_europe:boolean
gbyte_europe:float, default: 0.00
calls_internationnal:boolean
net_internationnal:boolean


# JoinTableMobilContract

## Model JoinTableMobilSimulationContract

belongs_to :join_table_mobil_contract
belongs_to :mobil_contract

rails g model JoinTableMobilContract savings:float

## Migration

t.belongs_to :mobil_simulation, index: true
t.belongs_to :mobil_contract, index: true
t.savings:float, default: 0.OO


# MobilSimulation

## Model MobilSimulation

has_many :join_table_mobil_simulation_contracts
has_many :mobil_contracts, through: :join_table_mobil_simulation_contracts
belongs_to :full_simulation

rails g model MobilSimulation name:string actual_price_paid:float mobil_cost_saved:float calls_france:boolean calls_europe:boolean gbyte_europe:float

## Migration

t.belongs_to :full_simulation, index: true
t.name:string, default: "Tléphone"
t.float :actual_price_paid
t.float :mobil_cost_saved
t.float :bundle_gbyte
t.boolean :calls_france
t.boolean :calls_europe
t.float :gbyte_europe



## Controller

rails g controller mobil_simulations index....

# FullSimulation

## Model FullSimulation

has_one :mobil_simulation

def only_one_mobil_simulation
end 



