(define_automaton "bsg_vanilla")

;; EXE stage functional units, FPU has 3-stage pipeline
(define_cpu_unit "bsg_vanilla_branch_alu" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_ld_st" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_mul_div" "bsg_vanilla")
(define_cpu_unit "bsg_vanilla_fpu1,bsg_vanilla_fpu2,bsg_vanilla_fpu3" "bsg_vanilla")


(define_insn_reservation "bsg_vanilla_arith" 1
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "unknown,arith,shift,slt,multi,logical,move"))
  "bsg_vanilla_branch_alu")

(define_insn_reservation "bsg_vanilla_branch" 1
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "branch"))
  "bsg_vanilla_branch_alu")

(define_insn_reservation "bsg_vanilla_jump" 1
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "jump,call"))
  "bsg_vanilla_branch_alu")

(define_insn_reservation "bsg_vanilla_load" 1
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "load,nop,const,auipc,fpload"))
  "bsg_vanilla_ld_st")

(define_insn_reservation "bsg_vanilla_store" 1
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "store,fpstore"))
  "bsg_vanilla_ld_st")

(define_insn_reservation "bsg_vanilla_imul" 32
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "imul,idiv"))
  "bsg_vanilla_mul_div*32")

(define_insn_reservation "bsg_vanilla_fpu" 3
  (and (eq_attr "tune" "bsg_vanilla")
       (eq_attr "type" "fadd,fmul,fdiv,fcvt,fcmp,fmove"))
  "bsg_vanilla_fpu1,bsg_vanilla_fpu2,bsg_vanilla_fpu3")

