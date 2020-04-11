(define_automaton "bsg_vanilla")

;; EXE stage functional units, FPU has 3-stage pipeline
(define_cpu_unit "bsg_vanilla_branch_alu" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_ld_st" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_mul_div" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_fpu" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_wb" "bsg_vanilla")

(define_insn_reservation "bsg_vanilla_arith" 2
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "unknown,arith,shift,slt,multi,logical,move"))
  "bsg_vanilla_branch_alu,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_branch" 2
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "branch"))
  "bsg_vanilla_branch_alu,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_jump" 2
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "jump,call"))
  "bsg_vanilla_branch_alu,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_load" 2
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "load,nop,const,auipc,fpload"))
  "bsg_vanilla_ld_st,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_store" 2
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "store,fpstore"))
  "bsg_vanilla_ld_st,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_imul" 33
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "imul,idiv"))
  "bsg_vanilla_mul_div*32,bsg_vanilla_wb")

(define_insn_reservation "bsg_vanilla_floats" 4
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "fadd,fmul,fdiv,fcvt,fcmp,fmove"))
  "bsg_vanilla_fpu,nothing*2,bsg_vanilla_wb")
