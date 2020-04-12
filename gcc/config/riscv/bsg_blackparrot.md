(define_automaton "bsg_blackparrot")

;; EXE stage functional units, FPU has 3-stage pipeline
(define_cpu_unit "bsg_blackparrot_alu" "bsg_blackparrot")
(define_cpu_unit "bsg_blackparrot_mem" "bsg_blackparrot")
(define_cpu_unit "bsg_blackparrot_mul_div" "bsg_blackparrot")
(define_cpu_unit "bsg_blackparrot_fpu" "bsg_blackparrot")

(define_insn_reservation "bsg_blackparrot_alu" 1
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "unknown,arith,shift,slt,multi,logical,move"))
  "bsg_blackparrot_alu")

(define_insn_reservation "bsg_blackparrot_branch" 1
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "branch"))
  "bsg_blackparrot_alu")

(define_insn_reservation "bsg_blackparrot_jump" 1
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "jump,call"))
  "bsg_blackparrot_alu")

(define_insn_reservation "bsg_blackparrot_load" 3
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "load,nop,const,auipc,fpload"))
  "bsg_blackparrot_mem,nothing*2")

(define_insn_reservation "bsg_blackparrot_store" 3
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "store,fpstore"))
  "bsg_blackparrot_mem,nothing*2")

(define_insn_reservation "bsg_blackparrot_imul" 4
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "imul,idiv"))
  "bsg_blackparrot_mul_div,nothing*3")

(define_insn_reservation "bsg_blackparrot_floats" 5
  (and (eq_attr "tune" "bsg_blackparrot")
       (eq_attr "type" "fadd,fmul,fdiv,fcvt,fcmp,fmove"))
  "bsg_blackparrot_fpu,nothing*4")
